---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Note sulla versione per ASP.NET e gli strumenti Web 2013.1 per Visual Studio 2012 Documenti Microsoft
author: rick-anderson
description: In questo documento viene descritto il rilascio di ASP.NET e Web Tools 2013.1 per Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d4aced4e77a150d52358c2d2513ff79e6594a9de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543574"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Note sulla versione di ASP.NET and Web Tools 2013.1 per Visual Studio 2012

da parte [di Microsoft](https://github.com/microsoft)

> In questo documento viene descritto il rilascio di ASP.NET e Web Tools 2013.1 per Visual Studio 2012.

## <a name="contents"></a>Sommario

- [Note di installazione](#install)
- [Requisiti software](#requirements)
- Nuove funzionalità in ASP.NET e Web Tools 2013.1 per Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modelli](#templates)

        - [ASP.NET modello MVC 5](#mvc5template)
        - [ASP.NET modello Web API 2](#apitemplate)
        - [Modelli di elemento](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET Scaffolding](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes

    - [ASP.NET Scaffolding](#issuescaffolding)

        - [MVC e API Web Scaffolding - ERRORE HTTP 404, non trovato](#404issue)
        - [Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento con scaffolding](#expressissue)
    - [ASP.NET Rasoio 3](#issuerazor)

        - [La visualizzazione del file cshtml con Sfoglia con o F5 causa un errore del server](#browseissue)
        - [Riscrittura URL e Tilde(](#rewriteissue)
    - [Modelli](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Note di installazione

[Installazione](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET e Web Tools 2013.1 per Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisiti software

È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per il Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nuove funzionalità in ASP.NET e Web Tools 2013.1 per Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Quando si esegue lo scaffolding dei controller e delle visualizzazioni MVC 5, il markup per le visualizzazioni utilizza [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modelli

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET modello MVC 5

È stato aggiunto un nuovo modello MVC 5. Fa riferimento ai pacchetti NuGet MVC 5 più recenti ed è possibile usare lo scaffolding per aggiungere controller e visualizzazioni.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET modello Web API 2

È stato aggiunto un nuovo modello di API Web 2.We added a new Web API 2 template. Fa riferimento ai pacchetti NuGet dell'API Web più recenti ed è possibile usare lo scaffolding per aggiungere controller e visualizzazioni.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modelli di elementi

Sono stati aggiunti nuovi modelli di elemento per le visualizzazioni MVC 5, le pagine Web (Razor 3) e i controller Web API 2. Installano i pacchetti NuGet correlati al progetto durante l'aggiunta di nuovi elementi.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Quando si esegue lo scaffolding di un controller MVC o API Web utilizzando Entity Framework, viene usato Framework 6. Per ulteriori informazioni su Entity Framework, vedere [cronologia delle versioni](https://msdn.com/data/jj574253)di Entity Framework .

È inoltre possibile scaricare e installare Entity Framework 6 Tools per Visual Studio 2012. Vedere [Get Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffolding è un framework di generazione di codice per applicazioni Web ASP.NET. Semplifica l'aggiunta di codice boilerplate al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, lo scaffolding era limitato aASP.NET progetti MVC. Con questo aggiornamento, è ora possibile utilizzare scaffolding per qualsiasi progetto ASP.NET, inclusi Web Form. Questo aggiornamento non supporta la generazione di pagine per un progetto Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo dipendenze MVC al progetto. Il supporto per la generazione di pagine per Web Form verrà aggiunto in un aggiornamento futuro.

Quando si usa lo scaffolding, si garantisce che tutte le dipendenze necessarie siano installate nel progetto. Ad esempio, se si inizia con un ASP.NET progetto Web Form e quindi si utilizza lo scaffolding per aggiungere un Controller API Web, i pacchetti E i riferimenti NuGet necessari vengono aggiunti automaticamente al progetto.

Per aggiungere lo scaffolding MVC a un progetto Web Form, aggiungere un **nuovo elemento con scaffolding** e selezionare **Dipendenze MVC 5** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo. Se si seleziona Minimal, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione Completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC.

Il supporto per lo scaffolding dei controller asincroni usa le nuove funzionalità asincrone di Entity Framework 6.Support for scaffolding async controllers uses the new async features from Entity Framework 6.

Per ulteriori informazioni ed esercitazioni, vedere [Cenni ASP.NET scaffolding](../2013/aspnet-scaffolding-overview.md). Queste esercitazioni illustrano lo scaffolding con Visual Studio 2013, ma sono applicabili anche ai ASP.NET e Web Tools 2013.1 per Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Con questo aggiornamento, Visual Studio 2012 supporta ora Razor 3 strumenti/editing.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 include un ricco set di nuove funzionalità descritte in dettaglio in [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet elimina la necessità per gli utenti di consentire in modo esplicito a NuGet di ripristinare i pacchetti mancanti. Quando si installa NuGet 2.7, gli utenti acconsentono implicitamente al ripristino automatico dei pacchetti mancanti. Gli utenti possono rifiutare esplicitamente il ripristino del pacchetto tramite le impostazioni NuGet in Visual Studio.Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio. Questa modifica semplifica il funzionamento del ripristino dei pacchetti.

## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e API Web Scaffolding - ERRORE HTTP 404, non trovato

Se si verifica un errore durante l'aggiunta di un elemento con scaffolding a un progetto, è possibile che il progetto venga lasciato in uno stato incoerente. Verrà eseguito il rollback di alcune delle modifiche apportate come scaffolding, ma non verrà eseguito il rollback di altre modifiche, ad esempio i pacchetti NuGet installati. Se viene eseguito il rollback delle modifiche alla configurazione di routing, gli utenti riceveranno un errore HTTP 404 durante lo spostamento agli elementi con scaffolding.

Per correggere questo errore per MVC, aggiungere un nuovo elemento con scaffolding e selezionare Dipendenze MVC 5 (minimo o completo). Questo processo aggiungerà tutte le modifiche necessarie al progetto.

Per correggere questo errore per l'API Web:

1. Aggiungere la classe WebApiConfig seguente al progetto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configurare WebApiConfig.Register\_nel metodo Application Start in Global.asax come segue:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento con scaffolding

Se Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di elemento con scaffolding con Entity Framework (ad esempio Web API 2 Controller con azioni, utilizzando Entity Framework), è possibile che Visual Studio Express non sia riuscito a caricare l'immagine nativa di un assembly dipendente da System.Web.Extensions.

Per risolvere questo problema, configurare Visual Studio Express per l'utilizzo con l'immagine MSIL di System.Web.Extensions:

1. Aprire il prompt dei comandi in modalità amministratore.
2. Andare a %ProgramFiles%, Microsoft Visual Studio 11.0, Common7, IDE o %ProgramFiles(x86)% .
3. Aprire VWDExpress.exe.config in un editor di testo.
4. Aggiungere la riga &lt;seguente&gt;/&lt;&gt; sotto l'elemento runtime di configurazione:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Riavviare Visual Studio Express 2012 per il Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Rasoio 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>La visualizzazione del file cshtml con Sfoglia con o F5 causa un errore del server

Quando si crea un progetto MVC 5 in Visual Studio 2012 (o aprire in Visual Studio 2012 un progetto MVC 5 creato in Visual Studio 2013) e si tenta di visualizzare un file cshtml utilizzando Sfoglia con o F5, verrà visualizzato un errore che indica - **Errore del Server nell'applicazione '/'.** Il server tenta di navigare`http://localhost:XXXX/Views/../XXXX.cshtml`

Per risolvere questo problema, modificare l'impostazione **Azione di avvio** nel progetto in **Pagina specifica**. Non è necessario fornire un valore per la pagina.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Dopo aver apportato questa modifica, selezionando F5`http://localhost:XXXX`si passa alla radice dell'applicazione ( ). Questo comportamento non corrisponde al comportamento per i progetti MVC 5 in Visual Studio 2013, in cui l'impostazione **Pagina corrente** avvia la pagina aperta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Riscrittura URL e Tilde(

Dopo l'ASP.NET'aggiornamento a ASP.NET Razor 3 o ASP.NET MVC 5, la notazione tilde() potrebbe non funzionare più correttamente se si utilizza la riscrittura URL. La riscrittura dell'URL influisce sulla notazione tilde(-) negli&gt; &lt;elementi&gt;HTML &lt;quali A/&gt;, &lt;SCRIPT/ , LINK/ e di conseguenza la tilde non è più mappata alla directory radice.

Se, ad esempio, si riscrivono le richieste &lt;di **asp.net/content** a **asp.net**, l'attributo href in A **/** href //content/"/&gt; viene risolto in **/content/content/** anziché . Per eliminare questa modifica, è possibile impostare il contesto **IIS\_WasUrlRewritten** su false in ogni pagina Web o **\_nell'applicazione BeginRequest** in Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modelli

Quando si creano ASP.NET progetti MVC con Visual Studio 2012 in Windows 8.1 o Windows Server 2012 R2, Visual Studio visualizza un messaggio di errore che indica "Configurazione Web [url] per ASP.NET 4.5 non riuscita."

![errore di configurazione](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Questo errore viene visualizzato perché Visual Studio 2012 non abilita la funzionalità ASP.NET 4.5 quando è installato in tali versioni di Windows. Per abilitare ASP.NET 4.5, eseguire la procedura descritta in [Attivare o disattivare le funzionalità](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)di Windows.

![attivare o disattivare le funzionalità di Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

In alternativa, è possibile abilitare ASP.NET 4.5 tramite la riga di comando.

1. Aprire il prompt dei comandi in modalità amministratore.
2. Eseguire il comando seguente per abilitare ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
