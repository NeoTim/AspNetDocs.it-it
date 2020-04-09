---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Creazione di ASP.NET progetti Web in Visual Studio 2013 Documenti Microsoft
author: tdykstra
description: In questo argomento vengono illustrate le opzioni per la creazione di ASP.NET progetti Web in Visual Studio 2013 con l'aggiornamento 3 Ecco alcune delle nuove funzionalità per lo sviluppo Web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676048"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Creazione di progetti Web ASP.NET in Visual Studio 2013

da parte [di Tom Dykstra](https://github.com/tdykstra)

> In questo argomento vengono illustrate le opzioni per la creazione di progetti Web ASP.NET in Visual Studio 2013 con aggiornamento 3
> 
> Ecco alcune delle nuove funzionalità per lo sviluppo Web rispetto alle versioni precedenti di Visual Studio:
> 
> - Un'interfaccia utente semplice per la creazione di progetti che offrono [supporto per più framework di ASP.NET](#add) (Web Form, MVC e API Web).
> - [ASP.NET Identity](#indauth), un nuovo sistema di appartenenze ASP.NET che funziona allo stesso modo in tutti i framework ASP.NET e funziona con software di hosting Web diverso da IIS.
> - L'uso di [Bootstrap](#bootstrap) per fornire funzionalità di progettazione reattiva e tema.
> - Nuove funzionalità per Web Form che in precedengo venivano offerte solo per MVC, ad esempio la [creazione automatica](#testproj) di progetti di test e un modello di sito [Intranet](#winauth).
> 
> Per informazioni su come creare progetti Web per Servizi cloud di Azure o Servizi mobili di Azure, vedere [Introduzione a Servizi cloud](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) di Azure e ASP.NET e Creazione di [un'app leaderboard con il back-end .NET di Servizi mobili di Azure.](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

Questo articolo si applica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [l'aggiornamento 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installato.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Progetti di applicazione Web e progetti di sito Web

ASP.NET offre la possibilità di scegliere tra due tipi di progetti Web: progetti di *applicazioni Web* e progetti di *siti Web.* Si consigliano progetti di applicazione Web per il nuovo sviluppo e questo articolo si applica solo ai progetti di applicazione Web. Per ulteriori informazioni, vedere [Progetti di applicazione Web e progetti](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) di sito Web in Visual Studio nel sito MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Panoramica della creazione di progetti di applicazioni Web

Nei passaggi seguenti viene illustrato come creare un progetto Web:

1. Fare clic su **Nuovo progetto** nella pagina **iniziale** o nel menu **File.**
2. Nella finestra di dialogo **Nuovo progetto** fare clic su **Web** nel riquadro sinistro e ASP.NET **applicazione Web** nel riquadro centrale.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image1.png)

    È possibile scegliere **Cloud** nel riquadro sinistro per creare un [servizio cloud di Azure,](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)un [servizio mobile](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)di Azure o un processo Web [di Azure.](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs) Questo argomento non tratta tali modelli.
3. Nel riquadro destro selezionare la casella di controllo **Aggiungi Application Insights al progetto** se si desidera il monitoraggio dell'integrità e dell'utilizzo per l'applicazione. Per altre informazioni, vedere [Monitorare le prestazioni di applicazioni Web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Specificare **nome**progetto , **Percorso**e altre opzioni, quindi fare clic su **OK**.

    Verrà visualizzata la finestra di dialogo **Nuovo progetto ASP.NET**.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Fare clic su un modello.

    ![Selezionare un modello:](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se si desidera aggiungere il supporto per framework aggiuntivi non inclusi nel modello, fare clic sulla casella di controllo appropriata. Nell'esempio illustrato è possibile aggiungere MVC e/o API Web a un progetto Web Form.

    ![Aggiungere framework](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se si desidera aggiungere un progetto di unit test, fare clic su **Aggiungi unit test**.

    ![Aggiungere unit test](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se si desidera un metodo di autenticazione diverso da quello fornito dal modello per impostazione predefinita, fare clic su **Modifica autenticazione**.

    ![Pulsante Configura autenticazione](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Finestra di dialogo Configura autenticazione](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Creare un'app Web o una macchina virtuale in AzureCreate a web app or virtual machine in Azure

Visual Studio include funzionalità che semplificano l'uso dei servizi di Azure per l'hosting di applicazioni Web.Visual Studio includes features that make it easy work with Azure services for hosting web applications. Ad esempio, è possibile eseguire tutte le operazioni seguenti direttamente dall'IDE di Visual Studio:For example, you can do all of the following right from the Visual Studio IDE:

- Creare e gestire app Web o macchine virtuali che rendono disponibile l'applicazione tramite Internet.
- Visualizzare i log creati dall'applicazione durante l'esecuzione nel cloud.
- Eseguire in modalità di debug in remoto mentre l'applicazione viene eseguita nel cloud.
- Visualizzare e gestire altri servizi di Azure, ad esempio i database SQL.

È possibile [creare gratuitamente un account Azure](https://www.windowsazure.com/pricing/free-trial/) che include gratuitamente servizi di base, ad esempio app Web, e se si è un abbonato MSDN è possibile [attivare i vantaggi](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) che offrono crediti mensili per servizi di Azure aggiuntivi. 

Per impostazione predefinita, la finestra di dialogo **Nuovo progetto ASP.NET** consente di creare un'app Web o una macchina virtuale per un nuovo progetto Web. Se non si desidera creare una nuova app Web o una macchina virtuale, deselezionare la casella di controllo **Host nel cloud.**

![Creare risorse remoteCreate remote resources](creating-web-projects-in-visual-studio/_static/image8.png)

La didascalia della casella di controllo potrebbe essere **Host nel cloud** o Crea risorse **remote**e in entrambi i casi l'effetto è lo stesso. Se si lascia selezionata la casella di controllo, Visual Studio crea un'app Web nel servizio app di Azure per impostazione predefinita. Se si preferisce, è possibile utilizzare la casella di riepilogo a discesa per modificarla in una **macchina virtuale.** Se non è già stato effettuato l'accesso ad Azure, vengono richieste le credenziali di Azure.If you're not already signed in to Azure, you're prompted for Azure credentials. Dopo l'accesso, una finestra di dialogo consente di configurare le risorse che verranno create da Visual Studio per il progetto. Nella figura seguente viene illustrata la finestra di dialogo per un'app Web. se si sceglie di creare una macchina virtuale, vengono visualizzate opzioni diverse.

![Configurare le impostazioni dell'app di AzureConfigure Azure App Settings](creating-web-projects-in-visual-studio/_static/image9.png)

Per altre informazioni su come usare questo processo per la creazione di risorse di Azure, vedere Introduzione ad [Azure e ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e Creazione di una macchina virtuale per un sito Web con Visual [Studio.](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/)

Nella parte restante di questo articolo vengono fornite ulteriori informazioni sui modelli disponibili e sulle relative opzioni. L'articolo introduce anche Bootstrap, il layout e il framework di tema utilizzato nei modelli.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modelli di progetto Web di Visual Studio 2013

Visual Studio usa i modelli per creare progetti Web.Visual Studio uses templates to create web projects. Un modello di progetto può creare file e cartelle nel nuovo progetto, installare pacchetti NuGet e fornire codice di esempio per un'applicazione funzionante rudimentale. I modelli implementano gli standard Web più recenti e hanno lo scopo di illustrare le procedure consigliate per l'utilizzo di tecnologie ASP.NET e di avviare un'implementazione ottimale per la creazione di un'applicazione personalizzata.

In Visual Studio 2013 sono disponibili le opzioni seguenti per i modelli di progetto Web per i progetti destinati a .NET 4.5 o versioni successive del framework .NET:

- [Modello vuoto](#empty)
- [Modello Web Form](#wf)
- [Modello MVC](#mvc)
- [Modello API Web](#webapi)
- [Modello applicazione a pagina singola](#spa)
- [Modello servizio mobile di AzureAzure Mobile Service template](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelli di Visual Studio 2012](#vs2012)

È anche possibile installare un'estensione di Visual Studio che fornisce un [modello di Facebook.](#facebook)

Per informazioni su come creare progetti destinati a .NET 4, vedere Modelli di [Visual Studio 2012](#vs2012) più avanti in questo argomento.

Per informazioni su come creare applicazioni di ASP.NET per i client mobili, vedere [Supporto dispositivi mobili in ASP.NET.](../../../mobile/overview.md)

<a id="empty"></a>
### <a name="empty-template"></a>Modello vuoto

Il modello Vuoto fornisce le cartelle e i file minimi per un'app Web ASP.NET, ad esempio un file di progetto (*csproj* o .* vbproj*) e un file *Web.config.* È possibile aggiungere il supporto per Web Form, MVC e/o API Web utilizzando le caselle di controllo sotto l'etichetta **Aggiungi cartelle e riferimenti di base per:.**

Per il modello Vuoto non sono disponibili opzioni di autenticazione. La funzionalità di autenticazione viene implementata nelle applicazioni di esempio e il modello vuoto non crea un'applicazione di esempio.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modello Web Form

Il framework Web Form offre le funzionalità seguenti che consentono di creare rapidamente siti Web ricchi di funzionalità di interfaccia utente e di accesso ai dati:

- Una finestra di progettazione WYSIWYG in Visual Studio.
- Controlli server che eseguono il rendering HTML e che è possibile personalizzare impostando proprietà e stili.
- Un ricco assortimento di controlli per l'accesso ai dati e la visualizzazione dei dati.
- Modello di eventi che espone eventi che è possibile programmare come si programerebbe un'applicazione client, ad esempio WPFWPF.
- Conservazione automatica dello stato (dati) tra le richieste HTTP.

In generale, la creazione di un'applicazione Web Form richiede meno sforzo di programmazione rispetto alla creazione della stessa applicazione utilizzando il framework MVC ASP.NET. Tuttavia, Web Form non è solo per lo sviluppo rapido di applicazioni. Ci sono molte applicazioni commerciali complesse e framework costruiti su Web Form.

Poiché una pagina Web Form e i controlli nella pagina generano automaticamente gran parte del markup inviato al browser, non si dispone del tipo di controllo granulare sul codice HTML che offre ASP.NET MVC. Il modello dichiarativo per la configurazione di pagine e controlli riduce al minimo la quantità di codice che è necessario scrivere, ma nasconde parte del comportamento di HTML e HTTP. Ad esempio, non è sempre possibile specificare esattamente quale markup potrebbe essere generato da un controllo.

Il framework Web Form non si presta con la stessa misura ASP.NET MVC a pratiche di sviluppo basate su modelli quali [lo sviluppo basato su test,](http://en.wikipedia.org/wiki/Test-driven_development)la [separazione delle preoccupazioni,](http://en.wikipedia.org/wiki/Separation_of_concerns)l'inversione [del controllo](http://en.wikipedia.org/wiki/Inversion_of_control)e l'inserimento delle [dipendenze.](http://en.wikipedia.org/wiki/Dependency_injection) Se si desidera scrivere codice fattorizzato in questo modo, è possibile; semplicemente non è così automatico come lo è nel framework di MVC ASP.NET. Il progetto [MVP di Web Forms ASP.NET](http://webformsmvp.com/) mostra un approccio che facilita la separazione dei problemi e della testabilità, pur mantenendo il rapido sviluppo che Web Form è stato costruito per fornire. Microsoft SharePoint è basato su Web Form MVP.

Il modello Web Form crea un'applicazione Web Form di esempio che utilizza [Bootstrap](#bootstrap) per fornire funzionalità di progettazione e tema reattive. Nella figura seguente viene illustrata la home page.

![Home page dell'app modello di Web Form](creating-web-projects-in-visual-studio/_static/image10.png)

Per ulteriori informazioni sui Web Form, vedere [ASP.NET Web Form](https://asp.net/web-forms). Per informazioni sulle operazioni eseguite dal modello Web Form, vedere [Creazione di un'applicazione Web Form di base mediante Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modello MVC

ASP.NET MVC è stato progettato per facilitare le procedure di sviluppo basate su modelli, ad esempio [lo sviluppo basato su test,](http://en.wikipedia.org/wiki/Test-driven_development)la [separazione delle preoccupazioni,](http://en.wikipedia.org/wiki/Separation_of_concerns) [l'inversione del controllo](http://en.wikipedia.org/wiki/Inversion_of_control)e l'inserimento delle [dipendenze.](http://en.wikipedia.org/wiki/Dependency_injection) Il framework incoraggia la separazione del livello di logica di business di un'applicazione Web dal relativo livello di presentazione. Dividendo l'applicazione in modelli (M), visualizzazioni (V) e controller (C), ASP.NET MVC può semplificare la gestione della complessità nelle applicazioni di grandi dimensioni.

Con ASP.NET MVC, si lavora più direttamente con HTML e HTTP che nei Web Form. Ad esempio, Web Form può mantenere automaticamente lo stato tra le richieste HTTP, ma è necessario codificare che in modo esplicito in MVC. Il vantaggio del modello MVC è che consente di assumere il controllo completo esattamente ciò che l'applicazione sta facendo e come si comporta nell'ambiente web. Lo svantaggio è che è necessario scrivere più codice.

MVC è stato progettato per essere estensibile, offrendo agli sviluppatori di alimentazione la possibilità di personalizzare il framework in base alle esigenze dell'applicazione. Inoltre, il codice sorgente ASP.NET MVC è disponibile in una licenza OSI.

Il modello MVC crea un'applicazione MVC 5 di esempio che utilizza [Bootstrap](#bootstrap) per fornire funzionalità di progettazione e tema reattive. Nella figura seguente viene illustrata la home page.

![Applicazione di esempio MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Per ulteriori informazioni su MVC, vedere [ASP.NET MVC](https://asp.net/mvc). Per informazioni su come selezionare il modello MVC 4, vedere modelli di [Visual Studio 2012](#vs2012) più avanti in questo articolo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modello API Web

Il modello di API Web crea un servizio Web di esempio basato sull'API Web, incluse le pagine della Guida dell'API basate su MVC.

ASP.NET Web API è un framework che consente di creare facilmente servizi HTTP in grado di raggiungere un ampio numero di client, inclusi browser e dispositivi mobili. ASP.NET API Web è una piattaforma ideale per la creazione di servizi RESTful in .NET Framework.

Il modello di API Web crea un servizio Web di esempio. Le illustrazioni seguenti mostrano pagine di aiuto di esempio.

![Pagina della Guida dell'API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Pagina della Guida dell'API Web per l'API GET](creating-web-projects-in-visual-studio/_static/image13.png)

Per ulteriori informazioni sull'API Web, vedere [ASP.NET API Web](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Single Page Application Template (in inglese)

Il modello di applicazione a pagina singola (SPA) crea un'applicazione di esempio che utilizza JavaScript, HTML 5 e [KnockoutJS](http://knockoutjs.com/) sul client e ASP.NET API Web sul server.

L'unica opzione di autenticazione per il modello SPA è [Singoli account utente](#indauth).

Nella figura seguente viene illustrato lo stato iniziale dell'applicazione di esempio compilata dal modello SPA.

![Applicazione di esempio SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Per informazioni su come creare un'applicazione utilizzando il modello SPA, vedere API Web - Servizi di [autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md).

Per altre informazioni su ASP.NET applicazioni a pagina singola e su modelli SPA aggiuntivi che usano framework JavaScript diversi da KnockoutJS, vedere le risorse seguenti:

- [ASP.NET applicazione](../../../single-page-application/index.md)a pagina singola .
- [Informazioni sulle funzionalità di protezione nel modello SPA per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applicazioni a pagina singola: creare app Web moderne e reattive con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modello Facebook

È possibile installare [un'estensione](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409)di Visual Studio che fornisce un modello di Facebook. Questo modello crea un'applicazione di esempio progettata per l'esecuzione all'interno del sito Web di Facebook.This template creates a sample application that is designed to run inside the Facebook web site. Si basa su ASP.NET MVC e utilizza l'API Web per la funzionalità di aggiornamento in tempo reale.

Non sono disponibili opzioni di autenticazione per il modello di Facebook perché le applicazioni di Facebook vengono eseguite all'interno del sito di Facebook e si basano sull'autenticazione di Facebook.

Per ulteriori informazioni sulle applicazioni ASP.NET Facebook, vedere [Aggiornamento dell'API MVC Facebook](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelli di Visual Studio 2012

La finestra di dialogo di creazione di progetti Web di Visual Studio 2013 non fornisce l'accesso ad alcuni modelli disponibili in Visual Studio 2012. Se si desidera utilizzare uno di questi modelli, è possibile fare clic sul nodo Visual Studio 2012 nel riquadro sinistro della finestra di dialogo Nuovo progetto di Visual Studio.

![Modelli di Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Il nodo **Visual Studio 2012** consente di scegliere i modelli Web seguenti a cui non si ha accesso nell'elenco predefinito dei modelli per Visual Studio 2013:

- Applicazione Web ASP.NET MVC 4
- Applicazione Web entità ASP.NET Dynamic Data
- Controllo server AJAX di ASP.NET
- ASP.NET di controllo server AJAX
- Controllo del server di ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap nei modelli di progetto web di Visual Studio 2013

I modelli di progetto di Visual Studio 2013 utilizzano [Bootstrap](http://getbootstrap.com/), un framework di layout e tema creato da Twitter. Bootstrap utilizza CSS3 per fornire un design reattivo, il che significa che i layout possono adattarsi dinamicamente alle diverse dimensioni delle finestre del browser. Ad esempio, in un'ampia finestra del browser la home page creata dal modello Web Form è simile alla figura seguente:

![Home page dell'app modello di Web Form](creating-web-projects-in-visual-studio/_static/image16.png)

Rendere la finestra più stretta e le colonne disposte orizzontalmente si spostano in disposizione verticale:

![Bootstrap disposizione colonna verticale](creating-web-projects-in-visual-studio/_static/image17.png)

Restringere la finestra un po 'di più, e il menu superiore orizzontale si trasforma in un'icona che è possibile fare clic per espandere in un menu orientato verticalmente:

![Icona del menu Bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu verticale Bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

È inoltre possibile utilizzare la funzione di tema di Bootstrap per effettuare facilmente un cambiamento nell'aspetto dell'applicazione. Ad esempio, è possibile eseguire la procedura seguente per modificare il tema.

1. Nel browser passare [http://Bootswatch.com](http://Bootswatch.com)a , scegliere un tema e quindi fare clic su **Scarica**. (Questo scarica *bootstrap.min.css* per impostazione predefinita; se si desidera esaminare il codice CSS, ottenere *bootstrap.css* anziché la versione unificata.)
2. Copiare il contenuto del file CSS scaricato.
3. In Visual Studio creare un nuovo file **di foglio** di stile denominato *bootstrap-theme.css* nella cartella *Content* e incollarvi il codice CSS scaricato.
4. Aprire *\_App Start/Bundle.config* e modificare *bootstrap.css* in *bootstrap-theme.css*.

Eseguire nuovamente il progetto e l'applicazione ha un nuovo aspetto. La figura seguente mostra l'effetto del tema Amelia:

![Tema Bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Molti temi Bootstrap sono disponibili, sia versioni gratuite e premium. Bootstrap offre anche una vasta gamma di componenti dell'interfaccia utente, come [menu a discesa,](http://twitter.github.io/bootstrap/components.html#dropdowns)gruppi di [pulsanti](http://twitter.github.io/bootstrap/components.html#buttonGroups)e [icone](http://twitter.github.io/bootstrap/base-css.html#images). Per ulteriori informazioni su Bootstrap, vedere [il sito Bootstrap](http://twitter.github.io/bootstrap/).

Se si utilizza la finestra di progettazione Web Form in Visual Studio, si noti che la finestra di progettazione non supporta CSS3, pertanto non mostra con precisione tutti gli effetti dei temi Bootstrap o modifiche di layout reattivo. Tuttavia, le pagine Web Form verranno visualizzate correttamente quando vengono visualizzate con un browser.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Aggiunta del supporto per framework aggiuntiviAdding Support for Additional Frameworks

Quando si seleziona un modello, la casella di controllo per i framework utilizzati dal modello viene selezionata automaticamente. Ad esempio, se si seleziona il modello **Web Form,** la casella di controllo **Web Form** è selezionata e non è possibile deselezionarla.

![Selezionare un modello:](creating-web-projects-in-visual-studio/_static/image21.png)

![Aggiungere framework](creating-web-projects-in-visual-studio/_static/image22.png)

È possibile selezionare la casella di controllo per un framework che non è incluso nel modello per aggiungere il supporto per tale framework quando viene creato il progetto. Ad esempio, per abilitare l'utilizzo di pagine *aspx* Web Form dopo aver selezionato il modello MVC, selezionare la casella di controllo **Web Form.** In alternativa, per abilitare MVC quando si utilizza il modello Web Form, fare clic sulla casella di controllo **MVC.** L'aggiunta di un framework consente il supporto in fase di progettazione e in fase di esecuzione. Ad esempio, se si aggiunge il supporto MVC a un progetto Web Form, sarà possibile eseguire lo scaffolding di controller e visualizzazioni.

Se si combinano Web Form e MVC in un progetto e si abilitano URL brevi in Web Form, potrebbero verificarsi problemi di routing [imprevisti](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in cui un URL ha più destinazioni possibili. Le route definite per prime avranno la precedenza. Ad esempio, se `Home` si dispone di un controller `http://contoso.com/home` e di una pagina *Home.aspx,* l'URL passerà a *Home.aspx* `EnableFriendlyUrls` se si chiama il metodo prima di chiamare il `MapRoute` metodo in *RouteConfig.cs*o lo stesso URL passerà alla visualizzazione predefinita per il `Home` controller se si chiama `MapRoute` prima `EnableFriendlyUrls`di .

L'aggiunta di un framework non comporta alcuna funzionalità dell'applicazione di esempio. Ad esempio, se si aggiunge il supporto Web Form dopo aver selezionato il modello MVC, non viene creato alcun file di home page *Default.aspx.* Vengono aggiunti solo le cartelle, i file e i riferimenti necessari per supportare il framework. Per questo motivo, l'aggiunta di framework non modifica le opzioni di autenticazione, implementate dal codice nelle applicazioni di esempio create dai modelli. Ad esempio, se si seleziona il modello vuoto e si aggiunge il supporto Web Form o MVC, il pulsante **Configura autenticazione** verrà comunque disabilitato.

Nelle sezioni seguenti viene descritto brevemente l'effetto di ogni casella di controllo.

### <a name="add-web-forms-support"></a>Aggiunta del supporto Web Form

Crea cartelle *\_App Data* e *Models* vuote e un file *Global.asax.* Questi sono già stati creati da tutti i modelli diversi dal modello vuoto, pertanto la selezione della casella di controllo Web Form non fa alcuna differenza per gli altri modelli.

Il modello Web Form abilita gli URL brevi per impostazione predefinita, ma quando si aggiunge il supporto Web Form ad altri modelli selezionando la casella di controllo Web Form gli URL descrittivi non vengono abilitati automaticamente.

### <a name="add-mvc-support"></a>Aggiungere il supporto MVC

Installa i pacchetti MVC, Razor e WebPages NuGet, crea cartelle *Dati app\_*, *Controller*, *Modelli*e *Visualizzazioni* vuote, crea la cartella *\_App Start* con *RouteConfig.cs* file e crea il file *Global.asax.*

### <a name="add-web-api-support"></a>Aggiungere il supporto API WebAdd Web API Support

Installa i pacchetti WebApi e Newtonsoft.Json NuGet, crea cartelle *App\_Data*, *Controllers*e *Models* vuote, crea la cartella *\_App Start* con *WebApiConfig.cs* file e crea il file *Global.asax.*

<a id="auth"></a>
## <a name="authentication-methods"></a>Authentication Methods

Visual Studio 2013 offre diverse opzioni di autenticazione per i modelli Web Form, MVC e API Web:

- [Nessuna autenticazione](#noauth)
- [Singoli account utente](#indauth) (ASP.NET Identità, precedentemente nota come appartenenza a ASP.NET)
- [Account dell'organizzazione](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [Autenticazione di Windows](#winauth) (Intranet)

![Finestra di dialogo Configura autenticazione](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Nessuna autenticazione

Se si seleziona **Nessuna autenticazione**, l'applicazione di esempio non conterrà pagine Web per l'accesso, né l'interfaccia utente che indica chi ha effettuato l'accesso, nessuna classe di entità per un database delle appartenenze e nessuna stringa di connessione per un database delle appartenenze.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Account utente singoli

Se si seleziona **Singoli account utente**, l'applicazione di esempio verrà configurata per l'utilizzo di ASP.NET'identità (precedentemente nota come appartenenza ASP.NET) per l'autenticazione utente. ASP.NET'identità consente a un utente di registrare un account, creando un nome utente e una password sul sito o accedendo con provider di social network come Facebook, Google, Account Microsoft o Twitter. L'archivio dati predefinito per i profili utente in ASP.NET Identity è un database LOCALDB di SQL Server, che è possibile distribuire in SQL ServerSQL Server o nel database SQL di Azure per il sito di produzione.

In Visual Studio 2013 queste funzionalità sono le stesse di Visual Studio 2012, ma il codice sottostante per il sistema di appartenenze ASP.NET è stato riscritto. I vantaggi della nuova base di codice includono quanto segue:

- Il nuovo sistema di appartenenze è basato su [OWIN](http://owin.org/) anziché sul modulo ASP.NET di autenticazione basata su form. Ciò significa che è possibile utilizzare lo stesso meccanismo di autenticazione se si utilizza Web Form o MVC in IIS, o si sta self-hosting API Web o SignalR.
- Il nuovo database delle appartenenze è gestito da Entity Framework Code First e tutte le tabelle sono rappresentate da classi di entità che è possibile modificare. Ciò significa che è possibile personalizzare facilmente lo schema del database e l'interfaccia utente Web correlata al profilo in base alle proprie esigenze ed è possibile distribuire facilmente gli aggiornamenti tramite migrazioni Code First.

Il nuovo sistema di appartenenze viene implementato automaticamente nei nuovi modelli e può essere implementato manualmente in qualsiasi progetto destinato a .NET 4.5 o versioni successive.

ASP.NET Identity è una buona scelta se si sta creando un sito Web Internet che è principalmente per i clienti esterni. Se l'organizzazione utilizza Active Directory o Office 365 e si desidera creare un progetto che consenta l'accesso Single Sign-On per dipendenti e partner commerciali, l'opzione **Account organizzazione** potrebbe essere una scelta migliore.

Per ulteriori informazioni sull'opzione Singoli account utente, vedere le risorse seguenti:

- [www.asp.net/identity](../../../identity/index.md). Documentazione sull'ASP.NET Identity sul sito web ASP.NET.
- [Creare un'app ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Viene inoltre illustrato come personalizzare i dati del profilo utente.
- [API Web - Servizi di autenticazione esterna](../../../web-api/overview/security/external-authentication-services.md)
- [Aggiunta di account di accesso esterni all'applicazione ASP.NET in Visual Studio 2013Adding External Logins to your ASP.NET application in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Account dell'organizzazione

Se si seleziona **Account organizzazione**, l'applicazione di esempio verrà configurata per l'utilizzo di Windows Identity Foundation (WIF) per l'autenticazione basata su account utente in Azure Active Directory (Azure AD, che include Office 365) o Windows Server Active Directory. Per altre informazioni, vedere Opzioni di [autenticazione dell'account aziendale](#orgauthoptions) più avanti in questo argomento.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticazione di Windows

Se si seleziona **Autenticazione di Windows**, l'applicazione di esempio verrà configurata per l'utilizzo del modulo IIS di autenticazione di Windows per l'autenticazione. L'applicazione visualizzerà il dominio e l'ID utente dell'account di Active Directory o del computer locale connesso a Windows ma non includerà la registrazione utente o l'interfaccia utente di accesso. Questa opzione è destinata ai siti Web Intranet.

In alternativa, è possibile creare un sito Intranet che utilizza l'autenticazione di Active Directory scegliendo [l'opzione locale in Account organizzazione](#orgauthonprem). L'opzione locale utilizza Windows Identity Foundation (WIF) anziché il modulo di autenticazione di Windows. Alcuni passaggi aggiuntivi sono necessari per configurare l'opzione locale, ma WIF abilita funzionalità che non sono disponibili con il modulo di autenticazione di Windows.Some additional steps are required in order to set up the On-Premises option, but WIF enables features that aren't available with the Windows Authentication module. Ad esempio, con WIF è possibile configurare l'accesso all'applicazione in Active Directory ed eseguire query sui dati della directory.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opzioni di autenticazione dell'account aziendale

La finestra di dialogo **Configura autenticazione** offre diverse opzioni per l'autenticazione dell'account di Azure Active Directory (Azure AD, che include Office 365) o l'autenticazione dell'account di Windows Server Active Directory (AD):The Configure Authentication dialog with you several options for Azure Active Directory (Azure AD, which includes Office 365) or Windows Server Active Directory (AD) account authentication:

- [Cloud - Organizzazione singola](#orgauthsingle) (Azure AD o AD che usa l'integrazione della directory con Azure AD)Cloud - Single Organization (Azure AD, or AD using directory integration with Azure AD)
- [Cloud - Multi Organization](#orgauthmulti) (Azure AD o AD che usa l'integrazione della directory con Azure AD)Cloud - Multi Organization (Azure AD, or AD using directory integration with Azure AD)
- [Locale](#orgauthonprem) (AD)

Se si vuole provare una delle opzioni di Azure AD ma non si dispone ancora di un account, [fare clic qui per iscriversi a un account Azure AD.](https://go.microsoft.com/fwlink/?LinkId=309942)

> [!NOTE]
> Se si sceglie una delle opzioni di Azure AD, il progetto richiede un database ed è necessario accedere a un account amministratore globale per il tenant di Azure AD. Immettere il nome e la password admin@contoso.onmicrosoft.comper un account aziendale, ad esempio , che dispone di autorizzazioni amministrative per il tenant di Azure AD.
> 
> **Non immettere le credenziali per un account contoso@hotmail.comMicrosoft, ad esempio , nella finestra di dialogo di accesso.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - Autenticazione a organizzazione singola

![Autenticazione a organizzazione singola](creating-web-projects-in-visual-studio/_static/image24.png)

Scegliere questa opzione se si vuole abilitare l'autenticazione per gli account utente definiti in un [tenant](https://technet.microsoft.com/library/jj573650.aspx)di Azure AD. Ad esempio, il sito è contoso.com e verrà reso disponibile ai dipendenti della società Contoso che fanno parte del tenant contoso.onmicrosoft.com. Non sarà possibile configurare Azure AD per consentire agli utenti di altri tenant di accedere all'applicazione.

#### <a name="domain"></a>Dominio

Immettere il dominio di Azure AD in cui si `contoso.onmicrosoft.com`vuole configurare l'applicazione, ad esempio: . Se si dispone di `contoso.onmicrosoft.com`un `contoso.com` dominio [personalizzato,](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)ad esempio , è possibile immetterlo qui.

#### <a name="access-level"></a>Livello di accesso

Se l'applicazione deve eseguire query o aggiornare le informazioni della directory utilizzando l'API Graph, scegliere **Single Sign-On, Read Directory Data** o Single **Sign-On, Read and Write Directory Data**. In caso contrario, scegliere **Single Sign-On**. Per altre informazioni, vedere Livelli di [accesso alle](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) applicazioni e [Uso dell'API Graph per eseguire query in Azure AD.](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx)

#### <a name="application-id-uri"></a>URI ID applicazione

Per impostazione predefinita, il modello crea automaticamente un URI ID applicazione aggiungendo il nome del progetto al dominio di Azure AD. Ad esempio, se il `Example` nome del `contoso.onmicrosoft.com`progetto è e `https://contoso.onmicrosoft.com/Example`il dominio è , l'URI dell'ID applicazione diventa . Se si desidera specificare manualmente l'URI dell'ID applicazione, espandere la sezione **Altre opzioni** e immettere l'URI dell'ID applicazione nella casella di testo. L'URI dell'ID `https://`applicazione deve iniziare con .

Per impostazione predefinita, se un'applicazione di cui è già stato eseguito il provisioning in Azure AD ha lo stesso URI dell'ID applicazione di quello usato da Visual Studio per il progetto, il progetto verrà connesso all'applicazione esistente anziché eseguirne il provisioning. Se si desidera eseguire il provisioning di una nuova applicazione in tal caso, deselezionare la casella di controllo **Sovrascrivi la voce dell'applicazione se ne esiste già una con lo stesso ID.**

Se la casella di controllo **Sovrascrivi** è deselezionata e Visual Studio trova un'applicazione esistente con lo stesso URI dell'ID applicazione, crea un nuovo URI aggiungendo un numero all'URI che stava per utilizzare. Si supponga, ad esempio, che il nome del progetto sia `Example`, si lascia vuota la casella di testo, si deseleziona la casella di controllo **Sovrascrivi** e il tenant di Azure AD dispone già di un'applicazione con URI `https://contoso.onmicrosoft.com/Example`. In tal caso, verrà eseguito il provisioning di `https://contoso.onmicrosoft.com/Example_20130619330903`una nuova applicazione con un URI ID applicazione come .

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisioning dell'applicazione in Azure AD

Per eseguire il provisioning dell'applicazione in Azure AD o connettere il progetto a un'applicazione esistente, Visual Studio necessita delle credenziali di un amministratore globale per il dominio. Quando si fa clic **su OK** nella finestra di dialogo **Configura autenticazione** , viene richiesto di immettere il nome utente e la password di un amministratore globale per il dominio specificato. Successivamente, quando si fa clic su **Crea progetto** nella finestra di dialogo Nuovo **progetto ASP.NET,** Visual Studio esegue il provisioning dell'applicazione in Azure AD. Si noti che come parte di questo processo Visual Studio incorpora i valori dei segreti client nel file Web.config che scadono un anno dopo la creazione.

Per informazioni su come creare applicazioni che usano l'autenticazione **Cloud - Single Organization,** vedere le risorse seguenti:For information about how to create applications that use Cloud - Single Organization authentication, see the following resources:

- [Autenticazione di AzureAzure Authentication](../2012/windows-azure-authentication.md)
- [Aggiunta del processo di accesso nell'applicazione Web tramite Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Sviluppo di app ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteggere ASP.NET API Web con Azure AD e Microsoft OWIN Components](https://msdn.microsoft.com/magazine/dn463788.aspx)

Le esercitazioni non sono ancora state aggiornate per Visual Studio 2013. alcune delle operazioni che le esercitazioni consentono di eseguire manualmente vengono automatizzate in Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - Autenticazione multi-organizzazione

![Autenticazione di più organizzazioni](creating-web-projects-in-visual-studio/_static/image25.png)

Scegliere questa opzione se si vuole abilitare l'autenticazione per gli account utente definiti in più [tenant](https://technet.microsoft.com/library/jj573650.aspx)di Azure AD. Ad esempio, il sito è contoso.com e verrà reso disponibile ai dipendenti della società Contoso che si trovano nel tenant contoso.onmicrosoft.com e ai dipendenti della società Fabrikam che si trovano nel fabrikam.onmicrosoft.com tenant.

Le impostazioni immesse e il passaggio di provisioning dell'applicazione sono simili [all'autenticazione a singola organizzazione.](#orgauthsingle)

Per informazioni su come creare applicazioni che usano l'autenticazione **Cloud - Multi Organization,** vedere le risorse seguenti:For information about how to create applications that use Cloud - Multi Organization authentication, see the following resources:

- [Facile integrazione di Web App &amp; con Azure Active Directory ASP.NET Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) nel blog del team di Active Directory.
- [Esercitazione sullo sviluppo di applicazioni Web multi-tenant con Azure AD.](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) L'esercitazione non è ancora stata aggiornata per Visual Studio 2013. alcuni di ciò che l'esercitazione ti indirizza a fare manualmente è automatizzato in Visual Studio 2013.
- [Devi registrarti con le tue organizzazioni multiple ASP.NET'app prima](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)di poter accedere . Blog di Vittorio Bertocci che spiega come risolvere un problema comune che le persone incontrano durante la creazione di un progetto che utilizza l'autenticazione multi-organizzazione.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticazione organizzativa locale

![Autenticazione organizzativa locale](creating-web-projects-in-visual-studio/_static/image26.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in Windows Server Active Directory (AD) e non si vuole usare Azure AD. È possibile utilizzare questa opzione per creare un sito Intranet o un sito Internet. Per un sito Internet, utilizzare Active Directory Federation Services (ADFS) per fornire l'accesso ad Active Directory. Per ulteriori informazioni, vedere [Utilizzare l'opzione di autenticazione dell'organizzazione locale (ADFS) con ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Per un sito Intranet, in alternativa è possibile scegliere [Autenticazione](#winauth) di Windows anziché questa opzione. Per l'opzione Autenticazione di Windows non è necessario fornire un URL del documento di metadati. Tuttavia, l'autenticazione di Windows non consente di controllare l'accesso alle applicazioni in Active Directory o di eseguire query sui dati della directory.

#### <a name="on-premises-authority"></a>Autorità locale

Immettere un URL che punti al documento di metadati. Il documento di metadati contiene le coordinate dell'autorità. L'applicazione utilizzerà tali coordinate per guidare il flusso di accesso Web.

#### <a name="application-id-uri"></a>URI ID applicazione

Fornire un URI univoco che ACTIVE può utilizzare per identificare questa applicazione o lasciare vuoto per consentire a Visual Studio di crearne uno.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

Questo documento ha fornito alcune informazioni di base per la creazione di un nuovo progetto Web ASP.NET in Visual Studio 2013. Per ulteriori informazioni sull'utilizzo di Visual [https://www.asp.net/visual-studio/](../../index.md)Studio per lo sviluppo Web, vedere .
