---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Esercitazioni e articoli consigliati su MVC | Microsoft Docs
author: Rick-Anderson
description: Questa pagina contiene i collegamenti alle esercitazioni di ASP.NET MVC e una sequenza consigliata per seguirli.
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: 7dc81cf09309194df4471fedfc74d4051f0fdb78
ms.sourcegitcommit: 8d34fb54e790cfba2d64097afc8276da5b22283e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2020
ms.locfileid: "85484217"
---
# <a name="mvc-recommended-tutorials-and-articles"></a>Esercitazioni e articoli consigliati su MVC

di [Rick Anderson](https://twitter.com/RickAndMSFT)

<a id="pwd"></a>
## <a name="getting-started"></a>Introduzione

- [Introduzione con ASP.NET MVC 5](introduction/getting-started.md) Questa serie di 11 parti è un punto di partenza valido.
- [Nozioni fondamentali su Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (corso video)
- [Introduzione a ASP.NET MVC](https://channel9.msdn.com/Series/Introduction-to-ASP-NET-MVC) di Jon Galloway e Christopher Harrison
- [Ciclo di vita di un'applicazione ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) Documento PDF che grafici il ciclo di vita di un'app ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Uso dei dati

- [Introduzione con EF 6 Code First con MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) La serie vincente di Tom Dykstra si immerge in EF.

<a id="wj"></a>
## <a name="security"></a>Sicurezza

- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Questa esercitazione popolare illustra la creazione di una semplice app e l'aggiunta di appartenenza e ruoli.
- [Creare un'App ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Questa esercitazione illustra come creare un'applicazione Web MVC 5 ASP.NET che consente agli utenti di accedere usando OAuth 2,0 con le credenziali di un provider di autenticazione esterno, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.
- [Creare un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Per prima cosa in una serie di identità, include il codice per [inviare nuovamente un collegamento di conferma](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [App ASP.NET MVC 5 con autenticazione a due fattori tramite SMS e posta elettronica](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Secondo in serie di identità.
- [Procedure consigliate per la distribuzione delle password e di altri dati sensibili in ASP.NET e in Servizio app di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` e il cookie di sicurezza, codice per richiedere che un utente disponga di un account di posta elettronica convalidato prima di poter eseguire l'accesso, come SignInManager verifica il requisito 2FA e altro ancora.
- [Conferma dell'account e recupero della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Fornisce informazioni dettagliate sull'identità non trovata in [creare un'app web ASP.NET MVC 5 sicura con accesso, conferma della posta elettronica e reimpostazione della password](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) , ad esempio come consentire agli utenti di reimpostare la password dimenticata.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Creare un'app web ASP.NET in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) Esercitazione breve e semplice per la distribuzione in Azure.
- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Prestazioni e debug

- [Eseguire la profilatura e il debug dell'app ASP.NET MVC con Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)

## <a name="aspnet-mvc-dropdownlistfor-with-selectlistitem"></a>ASP.NET MVC DropDownListFor con SelectListItem

Quando si usa l' <xref:System.Web.Mvc.Html.SelectExtensions.DropDownListFor%2A> Helper e si passa alla raccolta di `SelectListItem` da cui è popolata, la `DropdownListFor` modifica la raccolta passata dopo che è stata chiamata. `DropdownListFor``SelectListItems`consente di modificare le proprietà selezionate selezionando qualsiasi oggetto selezionato dall'elenco a discesa. Questo comporta un comportamento imprevisto.

<xref:System.Web.Mvc.Html.SelectExtensions.DropDownListFor%2A>,, <xref:System.Web.Mvc.Html.SelectExtensions.DropDownList%2A> <xref:System.Web.Mvc.Html.SelectExtensions.EnumDropDownListFor%2A> , <xref:System.Web.Mvc.Html.SelectExtensions.ListBox%2A> E <xref:System.Web.Mvc.Html.SelectExtensions.ListBoxFor%2A> aggiornano la proprietà selezionata di qualsiasi oggetto `IEnumerable<SelectListItem>` passato o trovato in ViewData.

La soluzione alternativa consiste nel creare enumerabili distinti, contenenti `SelectListItem` istanze distinte, per ogni proprietà nel modello.

Per ulteriori informazioni, vedere

* [DropdownListFor modifica la raccolta di SelectListItem passata](http://web.archive.org/web/20140902031437/http://aspnetwebstack.codeplex.com/workitem/1913)
* [GetSelectListWithDefaultValue modifica l'oggetto Select di IEnumerable <SelectListItem>](https://github.com/aspnet/AspNetWebStack/issues/271)