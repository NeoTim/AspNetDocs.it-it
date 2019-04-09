---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 è un framework per compilare applicazioni scalabili, basati su standard web usando i modelli di progettazione consolidati e la potenza di AS....
ms.author: riande
ms.date: 10/11/2018
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: ed25b2563f8c3f2d686affbcad4e2844289cb287
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59406756"
---
# <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

## <a name="whats-new-in-aspnet-mvc-5"></a>What ' s New in ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

I modelli di progetto Web MVC si integrano facilmente con l'esperienza di One ASP.NET. È possibile personalizzare il progetto MVC e configurare l'autenticazione mediante la creazione guidata progetto di One ASP.NET. Un'esercitazione introduttiva su ASP.NET MVC 5 reperibili [Introduzione a ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [come aggiornare un ASP.NET MVC 4 e un progetto API Web ASP.NET MVC 5 e API Web 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto MVC sono stati aggiornati per usare ASP.NET Identity per l'autenticazione e la gestione delle identità. Un'esercitazione con l'autenticazione di Facebook e Google e la nuova API di appartenenza reperibili [creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [distribuire un'app di App ASP.NET MCV sicura con L'appartenenza, OAuth e Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

Il modello di progetto MVC è stato aggiornato per usare [Bootstrap](http://getbootstrap.com/) per fornire un sofisticato e reattivo aspetto che è possibile personalizzare con facilità. Per altre informazioni, vedere [Bootstrap nei modelli di progetto web Visual Studio](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtri di autenticazione

[Filtri di autenticazione](http://www.dotnetcurry.com/showarticle.aspx?ID=957) sono un nuovo tipo di filtro in ASP.NET MVC che vengono eseguiti prima dei filtri di autorizzazione nella pipeline ASP.NET MVC e consentono di specificare l'autenticazione per la logica per ogni azione, per ogni controller o a livello globale per tutti i controller. Filtri di autenticazione elaborano credenziali nella richiesta e forniscono un'entità corrispondente. Filtri di autenticazione possono anche aggiungere richieste di autenticazione in risposta a richieste non autorizzate. Visualizzare [filtri di autenticazione ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtri di autenticazione in ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Override del filtro

È ora possibile ignorare i filtri da applicare a un metodo di azione specificato o un controller, specificando un' [eseguire l'override di filtro](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). I filtri di sostituzione specificano un set di tipi di filtro che non devono essere eseguiti per un determinato ambito (azione o controller). In questo modo è possibile configurare i filtri che si applicano a livello globale ma quindi escludere determinati filtri globali dell'applicazione a controller o azioni specifiche. Visualizzare [override del filtro nuove funzionalità in ASP.NET MVC 5 e API Web ASP.NET 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [come usare la funzionalità di esegue l'override di filtri di ASP.NET MVC 5](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), e [override del filtro in ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET MVC sono ora supportate [routing con attributi](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), grazie a un contributo da Tim McCall, l'autore di [AttributeRouting](https://github.com/mccalltd/AttributeRouting). Con il routing con attributi è possibile specificare le route annotando le azioni e controller.

## <a name="new-web-project-experience"></a>Nuova esperienza di progetto Web

Visual Studio migliorato l'esperienza di creazione di nuovi progetti web, a partire da Visual Studio 2013. Nel **nuovo progetto Web ASP.NET** finestra di dialogo è possibile selezionare il tipo di progetto si desidera, configura qualsiasi combinazione di tecnologie (Web Form, MVC, Web API), configurare le opzioni di autenticazione, aggiungere il supporto di Docker e aggiunta un progetto unit test.

![Nuovo progetto ASP.NET](mvc5/_static/new-aspnet-web-app-dialog.png)

La finestra di dialogo consente di modificare le opzioni di autenticazione predefinito per molti dei modelli. Ad esempio, quando si crea un progetto di Web Form ASP.NET è possibile selezionare una delle opzioni seguenti:

- Nessuna autenticazione
- Account utente individuali (appartenenza ASP.NET o log provider basati su social network in)
- Account aziendali o dell'istituto di istruzione (Active Directory in un'applicazione internet)
- Autenticazione di Windows (Active Directory in un'applicazione intranet)

![Opzioni di autenticazione](mvc5/_static/change-authentication-dialog.png)

Per altre informazioni sul processo per la creazione di progetti web, vedere [creazione di progetti Web ASP.NET in Visual Studio](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Per altre informazioni sulle opzioni di autenticazione, vedere [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Semplifica inoltre aggiungere il codice boilerplate per il progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio 2013, lo scaffolding era limitato ai progetti ASP.NET MVC. A partire da Visual Studio 2013, è possibile usare lo scaffolding per qualsiasi progetto ASP.NET, tra cui Web Form. Visual Studio non supporta attualmente la generazione di pagine per un progetto di Web Form, ma è comunque possibile usare lo scaffolding con Web Form mediante l'aggiunta di dipendenze MVC al progetto. Verrà aggiunto il supporto per la generazione di pagine per Web Form in una versione futura.

Quando si usa lo scaffolding, tutti necessari le dipendenze siano installate nel progetto. Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.

Per aggiungere lo scaffolding di MVC in un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **dipendenze MVC 5** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding di MVC; **Dipendenze minime** e **completo delle dipendenze**. Se si seleziona **dipendenze minime**, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona **completa le dipendenze**, vengono aggiunte le dipendenze minime, nonché i necessari file di contenuto per un progetto MVC.

![Aggiungi finestra di dialogo di scaffolding in Visual Studio](overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

Supporto per controller asincroni scaffolding Usa le funzionalità asincrone da Entity Framework 6.

Per altre informazioni ed esercitazioni, vedere [Panoramica di Scaffolding ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="get-help-and-report-issues"></a>Get Help e segnalare problemi

- [Problemi noti e l'elenco di modifiche di rilievo](../visual-studio/overview/2013/release-notes.md#knownissues)
- Ottenere assistenza e discutere ASP.NET MVC 5 nel [forum](https://forums.asp.net/1146.aspx)
- [Segnalare un bug di ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Effettuare una richiesta di funzionalità](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrade-from-aspnet-mvc-4"></a>Eseguire l'aggiornamento da ASP.NET MVC 4

Vedere [come aggiornare un ASP.NET MVC 4 e Web del progetto API ASP.NET MVC 5 e API Web 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
