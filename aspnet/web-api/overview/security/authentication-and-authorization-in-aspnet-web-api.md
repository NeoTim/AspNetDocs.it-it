---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticazione e autorizzazione nellASP.NETAPI Web Documenti Microsoft
author: MikeWasson
description: Viene fornita una panoramica generale dell'autenticazione e dell'autorizzazione nellASP.NETAPI Web.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676258"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentication and Authorization in ASP.NET Web API (in inglese)

da parte di [Mike Wasson](https://github.com/MikeWasson)

È stata creata un'API Web, ma ora si desidera controllarne l'accesso. In questa serie di articoli verranno esaminare alcune opzioni per la protezione di un'API Web da utenti non autorizzati. Questa serie riguarda sia l'autenticazione che l'autorizzazione.

- *L'autenticazione* conosce l'identità dell'utente. Ad esempio, Alice accede con il proprio nome utente e password e il server utilizza la password per autenticare Alice.
- *L'autorizzazione* decide se un utente è autorizzato a eseguire un'azione. Ad esempio, Alice dispone dell'autorizzazione per ottenere una risorsa ma non di creare una risorsa.

Il primo articolo della serie fornisce una panoramica generale dell'autenticazione e dell'autorizzazione in ASP.NETAPI Web. Altri argomenti descrivono scenari di autenticazione comuni per l'API Web.Other topics describe common authentication scenarios for Web API.

> [!NOTE]
> Grazie alle persone che hanno recensito questa serie e fornito feedback preziosi: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Authentication

L'API Web presuppone che l'autenticazione avvenga nell'host. Per l'hosting Web, l'host è IIS, che utilizza moduli HTTP per l'autenticazione. È possibile configurare il progetto per l'utilizzo di uno qualsiasi dei moduli di autenticazione incorporati in IIS o ASP.NET oppure scrivere un modulo HTTP personalizzato per eseguire l'autenticazione personalizzata.

Quando l'host autentica l'utente, crea un *oggetto principal*, ovvero un oggetto [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) che rappresenta il contesto di sicurezza in cui viene eseguito il codice. L'host associa l'entità al thread corrente impostando **Thread.CurrentPrincipal**. L'entità contiene un oggetto **Identity** associato che contiene informazioni sull'utente. Se l'utente è autenticato, la proprietà **Identity.IsAuthenticated** restituisce **true**. Per le richieste anonime, **IsAuthenticated** restituisce **false**. Per ulteriori informazioni sulle entità, vedere [Protezione basata](https://msdn.microsoft.com/library/shz8h065.aspx)sui ruoli .

### <a name="http-message-handlers-for-authentication"></a>HTTP Message Handlers for Authentication

Anziché utilizzare l'host per l'autenticazione, è possibile inserire la logica di autenticazione in un [gestore messaggi HTTP.](../advanced/http-message-handlers.md) In tal caso, il gestore messaggi esamina la richiesta HTTP e imposta l'entità.

Quando è consigliabile usare i gestori di messaggi per l'autenticazione? Ecco alcuni compromessi:

- Un modulo HTTP vede tutte le richieste che passano attraverso la pipeline ASP.NET. Un gestore messaggi visualizza solo le richieste indirizzate all'API Web.
- È possibile impostare gestori di messaggi per route, che consentono di applicare uno schema di autenticazione a una route specifica.
- I moduli HTTP sono specifici di IIS. I gestori di messaggi sono indipendenti dall'host, pertanto possono essere utilizzati sia con l'hosting Web che con l'hosting autonomo.
- I moduli HTTP partecipano alla registrazione IIS, al controllo e così via.
- I moduli HTTP vengono eseguiti in precedenza nella pipeline. Se si gestisce l'autenticazione in un gestore messaggi, l'entità non viene impostata fino a quando non viene eseguito il gestore. Inoltre, l'entità torna all'entità precedente quando la risposta esce dal gestore messaggi.

In genere, se non è necessario supportare l'hosting autonomo, un modulo HTTP è un'opzione migliore. Se è necessario supportare l'hosting autonomo, prendere in considerazione un gestore di messaggi.

### <a name="setting-the-principal"></a>Impostazione del principio

Se l'applicazione esegue una logica di autenticazione personalizzata, è necessario impostare l'entità su due posizioni:If your application performs any custom authentication logic, you must set the principal on two places:

- **Thread.CurrentPrincipal**. Questa proprietà è il modo standard per impostare l'entità del thread in .NET.
- **HttpContext.Current.User**. Questa proprietà è specifica di ASP.NET.

Il codice seguente mostra come impostare l'entità:The following code shows how to set the principal:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Per l'hosting web, è necessario impostare l'entità in entrambe le posizioni; in caso contrario, il contesto di sicurezza potrebbe diventare incoerente. Per self-hosting, tuttavia, **HttpContext.Current** è null. Per assicurarsi che il codice sia indipendente dall'host, pertanto verificare la presenza di null prima dell'assegnazione a **HttpContext.Current**, come illustrato.

## <a name="authorization"></a>Autorizzazione

L'autorizzazione viene eseguita più avanti nella pipeline, più vicino al controller. Ciò consente di effettuare scelte più granulari quando si concede l'accesso alle risorse.

- *I filtri di autorizzazione* vengono eseguiti prima dell'azione del controller. Se la richiesta non è autorizzata, il filtro restituisce una risposta di errore e l'azione non viene richiamata.
- All'interno di un'azione del controller, è possibile ottenere l'entità corrente dal **ApiController.User** proprietà. Ad esempio, è possibile filtrare un elenco di risorse in base al nome utente, restituendo solo le risorse che appartengono a tale utente.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Utilizzo dell'attributo [Authorize]

Api Web fornisce un filtro di autorizzazione incorporato, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Questo filtro controlla se l'utente è autenticato. In caso contrario, restituisce il codice di stato HTTP 401 (Non autorizzato), senza richiamare l'azione.

È possibile applicare il filtro a livello globale, a livello di controller o a livello di singole azioni.

**A livello globale:** per limitare l'accesso per ogni controller API Web, aggiungere il filtro **AuthorizeAttribute** all'elenco di filtri globali:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controller**: Per limitare l'accesso per un controller specifico, aggiungere il filtro come attributo al controller:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Azione**: Per limitare l'accesso per azioni specifiche, aggiungere l'attributo al metodo di azione:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

In alternativa, è possibile limitare il controller e quindi `[AllowAnonymous]` consentire l'accesso anonimo ad azioni specifiche, utilizzando l'attributo . Nell'esempio seguente `Post` il metodo è `Get` limitato, ma il metodo consente l'accesso anonimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Negli esempi precedenti, il filtro consente a qualsiasi utente autenticato di accedere ai metodi con restrizioni; vengono tenuti fuori solo gli utenti anonimi. È inoltre possibile limitare l'accesso a utenti specifici o a utenti in ruoli specifici:You can also limit access to specific users or to users in specific roles:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Il filtro **AuthorizeAttribute** per i controller API Web si trova nello spazio dei nomi **System.Web.Http.** Esiste un filtro simile per i controller MVC nello spazio dei nomi **System.Web.Mvc,** che non è compatibile con i controller API Web.

### <a name="custom-authorization-filters"></a>Filtri di autorizzazione personalizzati

Per scrivere un filtro di autorizzazione personalizzato, derivare da uno di questi tipi:To write a custom authorization filter, derive from one of these types:

- **AuthorizeAttribute**. Estendere questa classe per eseguire la logica di autorizzazione in base all'utente corrente e ai ruoli dell'utente.
- **AuthorizationFilterAttribute**. Estendere questa classe per eseguire la logica di autorizzazione sincrona che non è necessariamente basata sull'utente o sul ruolo corrente.
- **IAuthorizationFilter**. Implementare questa interfaccia per eseguire la logica di autorizzazione asincrona; ad esempio, se la logica di autorizzazione effettua chiamate di I/O o di rete asincrone. Se la logica di autorizzazione è associata alla CPU, è più semplice derivare da **AuthorizationFilterAttribute**, poiché non è necessario scrivere un metodo asincrono.

Nel diagramma seguente viene illustrata la gerarchia di classi per la classe **AuthorizeAttribute.The** following diagram shows the class hierarchy for the AuthorizeAttribute class.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorizzazione all'interno di un'azione del controllerAuthorization Inside a Controller Action

In alcuni casi, è possibile consentire una richiesta di procedere, ma modificare il comportamento in base all'entità. Ad esempio, le informazioni restituite potrebbero cambiare a seconda del ruolo dell'utente. All'interno di un metodo del controller, è possibile ottenere l'entità corrente dal **ApiController.User** proprietà.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
