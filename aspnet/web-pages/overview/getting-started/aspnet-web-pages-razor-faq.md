---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: Domande frequenti sulle pagine Web ASP.NET (Razor) Documenti Microsoft
author: Rick-Anderson
description: In questo articolo vengono elencate alcune domande frequenti su ASP.NET pagine Web (Razor) e WebMatrix. Versioni software utilizzate nell'esercitazione ASP.NET pagine Web (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: a312d1327bc88e721bf7fc7459e420e3f582c88d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543704"
---
# <a name="aspnet-web-pages-razor-faq"></a>Domande frequenti sulle pagine Web ASP.NET (Razor)

 di [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix non è più consigliato come ambiente di sviluppo integrato per ASP.NET pagine Web. Utilizzare [Visual Studio](xref:web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o Visual Studio [Code](https://code.visualstudio.com/).
>
> In questo articolo vengono elencate alcune domande frequenti su ASP.NET pagine Web (Razor) e WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Questa esercitazione funziona anche con ASP.NET Pagine Web 2, WebMatrix 2 e Visual Studio 2012.

- [Qual è la differenza tra ASP.NET pagine Web, Web Form ASP.NET e ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [È necessario WebMatrix per lavorare con le pagine Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [È possibile utilizzare ASP.NET controlli Web Form in una pagina Web Page?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [È possibile distribuire un sito di ASP.NET Web Pages senza utilizzare WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [È necessario utilizzare l'helper WebSecurity per supportare gli account di accesso?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET pagine Web supporta HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Posso usare JavaScript e jQuery con le pagine Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Risorse aggiuntive](#AdditionalResources)

Per domande su errori e altri problemi, vedere la Guida alla risoluzione dei problemi relativi alle [pagine Web (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)ASP.NET .

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Qual è la differenza tra ASP.NET pagine Web, Web Form ASP.NET e ASP.NET MVC?

Tutte e tre sono ASP.NET tecnologie per la creazione di applicazioni web dinamiche:

- ASP.NET Web Pages è incentrata sull'aggiunta di codice dinamico (lato server) e sull'accesso al database alle pagine HTML e sulle funzionalità di sintassi semplice e leggera.
- ASP.NET Web Form è basato su un modello a oggetti della pagina e sui controlli di tipo finestra tradizionali (pulsanti, elenchi e così via). Web Form utilizza un modello basato su eventi familiare a coloro che hanno lavorato con lo sviluppo basato su client (Windows Form).
- ASP.NET MVC implementa il modello model-view-controller per ASP.NET. L'accento è posto sulla "separazione delle preoccupazioni" (elaborazione, dati e livelli dell'interfaccia utente).

Tutti e tre i framework sono pienamente supportati e continuano a essere sviluppati dal team ASP.NET. In generale, la scelta del framework da utilizzare dipende dal background e dall'esperienza con ASP.NET.

ASP.NET pagine Web in particolare è stato progettato per rendere più facile per le persone che già conoscono HTML per aggiungere l'elaborazione del server alle loro pagine. È una buona scelta per studenti, hobbisti, persone in generale che sono nuovi alla programmazione. Può anche essere una buona scelta per gli sviluppatori che hanno esperienza con non-ASP.NET tecnologie web.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>È necessario WebMatrix per lavorare con le pagine Web?

No. WebMatrix non è più consigliato come ambiente di sviluppo integrato per ASP.NET pagine Web. Utilizzare [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o Visual Studio [Code](https://code.visualstudio.com/).

Se non si desidera utilizzare Visual Studio o Visual Studio Code, è possibile installare i prodotti dei componenti singolarmente utilizzando [L'Installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Sono necessari i seguenti prodotti:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (che installa anche il framework di pagine Web ASP.NET)
- IIS Express (server Web)
- Microsoft SQL Server Compact 4.0 (il database)

È possibile utilizzare un editor di testo per modificare le pagine *con estensione cshtml* (o *vbhtml).*

La gestione dei database di SQL Server Compact (file*con estensione sdf)* senza uno strumento è un po' più difficile. Visual Studio contiene strumenti per la gestione dei database *con estensione sdf.* È anche possibile eseguire comandi SQL nel codice per eseguire molte attività di gestione di SQL Server.You can also run SQL commands in code to perform many SQL Server management tasks.

Per testare le pagine *con estensione cshtml* senza utilizzare un ambiente di sviluppo integrato (IDE), è possibile distribuirle in un server attivo. Vedere [È possibile distribuire un sito di ASP.NET pagine Web senza utilizzare WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)

### <a name="running-iis-express-without-using-an-ide"></a>Esecuzione di IIS Express senza utilizzare un IDE

Se si installa IIS Express nel computer come server Web, è possibile utilizzarlo per testare le pagine. È possibile eseguire IIS Express dalla riga di comando e associarlo a un numero di porta specifico. È quindi possibile specificare tale porta quando si richiedono file *con estensione cshtml* nel browser.

In Windows, aprire un prompt dei comandi con privilegi di amministratore e passare a *C:* . (Per i sistemi a 64 bit, utilizzare la cartella *C:* Quindi immettere il comando seguente, utilizzando il percorso effettivo del sito:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

È possibile utilizzare qualsiasi numero di porta che non è già riservato da un altro processo. (I numeri di porta superiori a 1024 sono in genere gratuiti.) Per `path` il valore, utilizzare il percorso della cartella del sito Web in cui si trovano i file *con estensione cshtml.*

Dopo aver eseguito questo comando per configurare IIS Express per la gestione delle pagine, è possibile aprire un browser e individuare un file *con estensione cshtml.* Utilizzare un URL simile al seguente:

`http://localhost:35896/default.cshtml`

Per informazioni sulle opzioni della riga `iisexpress.exe /?` di comando di IIS Express, immettere nella riga di comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>È possibile utilizzare ASP.NET controlli Web Form in una pagina Web Page?

No. I controlli Web Form, ad esempio il controllo [CheckBox,](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) i [controlli di convalida](https://msdn.microsoft.com/library/bwd43d0x)e il controllo [GridView,](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) funzionano solo nelle pagine Web Form (file*con estensione aspx).* Questi controlli richiedono il framework della pagina Web Form.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>È possibile distribuire un sito di ASP.NET Web Pages senza utilizzare WebMatrix?

Sì. È possibile copiare manualmente i file del sito Web in un server (in genere tramite FTP). Se si esegue una copia manuale, è inoltre necessario copiare i file che supportano SQL Server Compact (il database). Per informazioni dettagliate, vedere la voce di blog [Distribuzione di applicazioni di pagine Web senza uno strumento](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>È necessario utilizzare l'helper WebSecurity per supportare gli account di accesso?

No. Il `SimpleMembership` provider che fa parte di ASP.NET pagine Web è un'opzione. Sono disponibili anche i provider di protezione che fanno parte di ASP.NET (che potrebbe essere utilizzato per lavorare in Web Form). Ad esempio, è possibile utilizzare l'autenticazione basata su form in ASP.NET pagine Web come in Web Form. Per un esempio di utilizzo dell'autenticazione basata su form, vedere l'articolo del supporto tecnico Microsoft [How To Implement Forms-Based Authentication in Your ASP.NET Application by Using C'è.NET](https://support.microsoft.com/kb/301240). Per scaricare un semplice esempio, vedere [ASP.NET versione di "Login &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Per informazioni su come utilizzare l'autenticazione di Windows, vedere il post di blog [Utilizzo dell'autenticazione](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298)di Windows in ASP.NET pagine Web .

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET pagine Web supporta HTML5?

Sì. Le pagine create con ASP.NET pagine Web (pagine*con estensione cshtml* o *vbhtml)* sono essenzialmente pagine HTML che contengono anche codice eseguito sul server, prima del rendering della pagina. Finché il browser dell'utente supporta HTML5, è possibile utilizzare gli elementi HTML5 in una pagina *.cshtml* o *.vbhtml.*

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Posso usare JavaScript e jQuery con le pagine Web?

Certo. Le pagine create con ASP.NET pagine Web (pagine *.cshtml* o *.vbhtml)* sono solo pagine HTML con codice server. Pertanto, tutto ciò che è possibile fare in una normale pagina HTML utilizzando JavaScript o jQuery si può fare anche in una pagina *.cshtml* o *.vbhtml.*

Il modello **Starter Site** in WebMatrix contiene numerose librerie jQuery. Se si crea un sito utilizzando tale modello, la cartella *Script* contiene una libreria principale jQuery (*jquery-1.6.2.js)* e librerie per la convalida jQuery (*jquery.validate.js*, e così via).

Ecco alcuni post di blog che illustrano come usare jQuery con ASP.NET pagine Web:

- [Aggiunta di bontà jQuery a ASP.NET pagine Web utilizzando WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) di Rachel Appel
- [5 min: WebMatrix , jQuery UI , json e modelli jQuery](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) di Jonas Eriksson
- [WebMatrix e jQuery Moduli](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) di Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Forum WebMatrix e pagine Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) sul sito Web ASP.NET
