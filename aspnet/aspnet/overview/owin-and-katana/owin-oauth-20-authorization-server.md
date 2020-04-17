---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Server di autorizzazione OWin OAuth 2.0 Documenti Microsoft
author: hongyes
description: Questo tutorial ti guiderà su come implementare un server di autorizzazione OAuth 2.0 usando il middleware OAuth OWIN. Questo è un tutorial avanzato che solo outlin...
ms.author: riande
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: d758fa2639d10e1b7be8d87c5d1f7adb7e292ac7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540404"
---
<a name="owin-oauth-20-authorization-server"></a>Server di autorizzazione OAuth 2.0 OWIN
====================
di [Hongye Sun](https://github.com/hongyes) e [Praburaj Thiagarajan](https://github.com/Praburaj)

> Questo tutorial ti guiderà su come implementare un server di autorizzazione OAuth 2.0 usando il middleware OAuth OWIN. Si tratta di un'esercitazione avanzata che descrive solo i passaggi per creare un server di autorizzazione OAuth 2.0 OWIN. Questo non è un tutorial passo dopo passo. [Scaricare il codice di esempio](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
>
> > [!NOTE]
> > Questa struttura non deve essere utilizzata per la creazione di un'app di produzione sicura. Questa esercitazione ha lo scopo di fornire solo una struttura su come implementare un server di autorizzazione OAuth 2.0 utilizzando il middleware OAuth OAuth OWIN.
>
>
> ## <a name="software-versions"></a>Versioni software
>
> | **Mostrato nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) | [Visual Studio 2013 Express per desktop](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express). Visual Studio 2012 con l'aggiornamento più recente dovrebbe funzionare, ma l'esercitazione non è stata testata con esso e alcune selezioni di menu e finestre di dialogo sono diverse. |
> | .NET 4.5 |  |
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Se hai domande che non sono direttamente correlate al tutorial, puoi pubblicarle su [Katana Project su GitHub](https://github.com/aspnet/AspNetKatana/). Per domande e commenti relativi al tutorial stesso, vedere la sezione commenti nella parte inferiore della pagina.


Il [framework OAuth 2.0](http://tools.ietf.org/html/rfc6749) consente a un'app di terze parti di ottenere un accesso limitato a un servizio HTTP. Anziché utilizzare le credenziali del proprietario della risorsa per accedere a una risorsa protetta, il client ottiene un token di accesso, ovvero una stringa che indica un ambito, una durata e altri attributi di accesso specifici. I token di accesso vengono rilasciati ai client di terze parti da un server di autorizzazione con l'approvazione del proprietario della risorsa.

Questo tutorial tratterà:

- Come creare un server di autorizzazione per supportare quattro tipi di concessione di autorizzazione e token di aggiornamento:How to create an authorization server to support four authorization grant types and refresh tokens:
    - Concessione del codice di autorizzazione
    - Concessione implicita
    - Concessione credenziali password proprietario risorsa
    - Concessione credenziali client
- Creazione di un server di risorse protetto da un token di accesso.
- Creazione di client OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) o [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)gratuito, come indicato in **Versioni software** nella parte superiore della pagina.
- Familiarità con OWIN. Vedere [Introduzione al progetto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) e Novità di [OWIN e Katana](index.md).
- Familiarità con la terminologia [OAuth,](http://tools.ietf.org/html/rfc6749) inclusi [Ruoli,](http://tools.ietf.org/html/rfc6749#section-1.1) [Flusso di protocollo](http://tools.ietf.org/html/rfc6749#section-1.2)e [Concessione autorizzazione](http://tools.ietf.org/html/rfc6749#section-1.3). [L'introduzione a OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) fornisce una buona introduzione.

## <a name="create-an-authorization-server"></a>Creare un server di autorizzazioneCreate an Authorization Server

In questa esercitazione verrà illustrato come utilizzare [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) e MVC ASP.NET per creare un server di autorizzazione. Speriamo di fornire presto un download per l'esempio completato, come questa esercitazione non include ogni passaggio. Creare innanzitutto un'app Web vuota denominata *AuthorizationServer* e installare i pacchetti seguenti:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (O qualsiasi altro pacchetto di login sociale come Microsoft.Owin.Security.Facebook)

Aggiungere una [classe OWIN Startup](owin-startup-class-detection.md) nella cartella radice del progetto denominata *Startup*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Creare una cartella *di avvio\_dell'app.* Selezionare la cartella *Di avvio\_dell'app* e utilizzare Maiusc-ALT-A per aggiungere la versione scaricata del file *AuthorizationServer,App\_Start,Startup.Auth.cs.*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Il codice precedente abilita i cookie di accesso dell'applicazione/esterno e l'autenticazione di Google, che vengono utilizzati dal server di autorizzazione stesso per gestire gli account.

Il `UseOAuthAuthorizationServer` metodo di estensione consiste nell'impostare il server di autorizzazione. Le opzioni di configurazione sono:

- `AuthorizeEndpointPath`: il percorso della richiesta in cui le applicazioni client reindirizzeranno l'agente utente per ottenere il consenso degli utenti per l'emissione di un token o di un codice. Deve iniziare con una barra iniziale, ad esempio , "`/Authorize`".
- `TokenEndpointPath`: le applicazioni client del percorso della richiesta comunicano direttamente per ottenere il token di accesso. Deve iniziare con una barra iniziale, ad esempio "/Token". Se al client viene rilasciato un [segreto client,\_](http://tools.ietf.org/html/rfc6749#appendix-A.2)è necessario fornirlo a questo endpoint.
- `ApplicationCanDisplayErrors`: impostare su `true` se l'applicazione Web desidera generare `/Authorize` una pagina di errore personalizzata per gli errori di convalida client nell'endpoint. Questa operazione è necessaria solo per i casi in cui il browser `client_id` non `redirect_uri` viene reindirizzato all'applicazione client, ad esempio quando il o non sono corretti. L'endpoint `/Authorize` dovrebbe aspettarsi di vedere l'"oauth. Errore", "oauth. ErrorDescription" e "oauth. ErrorUri" vengono aggiunte all'ambiente OWIN.

    > [!NOTE]
    > Se non true, il server di autorizzazione restituirà una pagina di errore predefinita con i dettagli dell'errore.
- `AllowInsecureHttp`: True per consentire l'accesso alle richieste di autorizzazione `redirect_uri` e token sugli indirizzi URI HTTP e per consentire ai parametri di richiesta di autorizzazione in ingresso di avere indirizzi URI HTTP.

    > [!WARNING]
    > Sicurezza - Questo è solo per lo sviluppo.
- `Provider`: oggetto fornito dall'applicazione per elaborare gli eventi generati dal middleware del server di autorizzazione. L'applicazione può implementare completamente l'interfaccia `OAuthAuthorizationServerProvider` o può creare un'istanza di e assegnare i delegati necessari per i flussi OAuth supportati da questo server.
- `AuthorizationCodeProvider`: produce un codice di autorizzazione monouso da restituire all'applicazione client. Affinché il server OAuth **MUST** sia protetto, `AuthorizationCodeProvider` l'applicazione DEVE `OnCreate/OnCreateAsync` fornire un'istanza in cui `OnReceive/OnReceiveAsync`il token prodotto dall'evento è considerato valido per una sola chiamata a .
- `RefreshTokenProvider`: produce un token di aggiornamento che può essere utilizzato per produrre un nuovo token di accesso quando necessario. Se non viene specificato, il server di `/Token` autorizzazione non restituirà i token di aggiornamento dall'endpoint.

## <a name="account-management"></a>Gestione degli account

OAuth non si preoccupa di dove o come gestire le informazioni dell'account utente. È [ASP.NET'identità](../../../identity/index.md) che ne è responsabile. In questo tutorial, semplificheremo il codice di gestione dell'account e ci assicureremo che l'utente possa effettuare l'accesso utilizzando il middleware dei cookie OWIN. Di seguito è riportato `AccountController`il codice di esempio semplificato per il :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`viene utilizzato per convalidare il client con il relativo URL di reindirizzamento registrato. `ValidateClientAuthentication`controlla l'intestazione dello schema di base e il corpo del modulo per ottenere le credenziali del client.

La pagina di login è mostrata di seguito:

![](owin-oauth-20-authorization-server/_static/image1.png)

Esamina ora la sezione OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) di IETF.

**Il provider** (nella tabella seguente) è [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Provider, che è `OAuthAuthorizationServerProvider`di tipo , che contiene tutti gli eventi del server OAuth.

| Passaggi del flusso dalla sezione Concessione codice di autorizzazione | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) Il client avvia il flusso indirizzando l'agente utente del proprietario della risorsa all'endpoint di autorizzazione. Il client include l'identificatore client, l'ambito richiesto, lo stato locale e un URI di reindirizzamento a cui il server di autorizzazione invierà l'agente utente una volta concesso (o negato l'accesso). | Provider Provider Provider.Match-Endpoint.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) Il server di autorizzazione autentica il proprietario della risorsa (tramite l'agente utente) e stabilisce se il proprietario della risorsa concede o nega la richiesta di accesso del client. | **Se l'utente concede l'accesso&gt; &lt;** Provider Provider.MatchEndpoint.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) Supponendo che il proprietario della risorsa conceda l'accesso, il server di autorizzazione reindirizza l'agente utente al client utilizzando l'URI di reindirizzamento fornito in precedenza (nella richiesta o durante la registrazione del client). ... |  |
|  |  |
| (D) Il client richiede un token di accesso dall'endpoint del token del server di autorizzazione includendo il codice di autorizzazione ricevuto nel passaggio precedente. Quando si effettua la richiesta, il client esegue l'autenticazione con il server di autorizzazione. Il client include l'URI di reindirizzamento utilizzato per ottenere il codice di autorizzazione per la verifica. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Di seguito `AuthorizationCodeProvider.CreateAsync` è `ReceiveAsync` illustrata un'implementazione di esempio per e per controllare la creazione e la convalida del codice di autorizzazione.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Il codice precedente usa un dizionario simultaneo in memoria per archiviare il codice e il ticket di identità e ripristinare l'identità dopo aver ricevuto il codice. In un'applicazione reale, verrebbe sostituito da un archivio dati persistente. L'endpoint di autorizzazione consente al proprietario della risorsa di concedere l'accesso al client. In genere, è necessaria un'interfaccia utente per consentire all'utente di fare clic su un pulsante e confermare la concessione. Il middleware OAuth OWIN consente al codice dell'applicazione di gestire l'endpoint di autorizzazione. Nell'app di esempio viene usato `OAuthController` un controller MVC chiamato per gestirlo. Di seguito è riportata l'implementazione di esempio:Here is the sample implementation:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

L'azione `Authorize` controllerà innanzitutto se l'utente ha effettuato l'accesso al server di autorizzazione. In caso contrario, il middleware di autenticazione sfida il chiamante per l'autenticazione utilizzando il cookie "Applicazione" e reindirizza alla pagina di accesso. (Vedere il codice evidenziato sopra.) Se l'utente ha effettuato l'accesso, eseguirà il rendering della visualizzazione Autorizza, come illustrato di seguito:

![](owin-oauth-20-authorization-server/_static/image2.png)

Se il pulsante **Concedi** è selezionato, l'azione `Authorize` creerà una nuova identità "Bearer" e accederà con esso. Verrà attivato il server di autorizzazione per generare un token di connessione e inviarlo al client con payload JSON.

### <a name="implicit-grant"></a>Concessione implicita

Fare riferimento alla sezione Concessione [implicita](http://tools.ietf.org/html/rfc6749#section-4.2) OAuth 2 di IETF.

 Il flusso [di concessione implicita](http://tools.ietf.org/html/rfc6749#section-4.2) illustrato nella Figura 4 è il flusso e il mapping che segue il middleware OAuth OWIN.

| Passaggi del flusso dalla sezione Concessione implicita | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) Il client avvia il flusso indirizzando l'agente utente del proprietario della risorsa all'endpoint di autorizzazione. Il client include l'identificatore client, l'ambito richiesto, lo stato locale e un URI di reindirizzamento a cui il server di autorizzazione invierà l'agente utente una volta concesso (o negato l'accesso). | Provider Provider Provider.Match-Endpoint.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) Il server di autorizzazione autentica il proprietario della risorsa (tramite l'agente utente) e stabilisce se il proprietario della risorsa concede o nega la richiesta di accesso del client. | **Se l'utente concede l'accesso&gt; &lt;** Provider Provider.MatchEndpoint.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) Supponendo che il proprietario della risorsa conceda l'accesso, il server di autorizzazione reindirizza l'agente utente al client utilizzando l'URI di reindirizzamento fornito in precedenza (nella richiesta o durante la registrazione del client). ... |  |
|  |  |
| (D) Il client richiede un token di accesso dall'endpoint del token del server di autorizzazione includendo il codice di autorizzazione ricevuto nel passaggio precedente. Quando si effettua la richiesta, il client esegue l'autenticazione con il server di autorizzazione. Il client include l'URI di reindirizzamento utilizzato per ottenere il codice di autorizzazione per la verifica. |  |

Poiché abbiamo già implementato`OAuthController.Authorize` l'endpoint di autorizzazione (azione) per la concessione del codice di autorizzazione, abilita automaticamente anche il flusso implicito. Nota: `Provider.ValidateClientRedirectUri` viene utilizzato per convalidare l'ID client con il relativo URL di reindirizzamento, che protegge il flusso di concessione implicita dall'invio del token di accesso a client dannosi[(Attacco man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Concessione credenziali password proprietario risorsa

Fare riferimento alla sezione OAuth 2 [Resource Owner Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) di IETF.

 Il flusso [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) mostrato nella Figura 5 è il flusso e il mapping che segue il middleware OWIN OAuth.

| Passaggi del flusso dalla sezione Credenziali password proprietario risorsa | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) Il proprietario della risorsa fornisce al client il nome utente e la password. |  |
|  |  |
| (B) Il client richiede un token di accesso dall'endpoint del token del server di autorizzazione includendo le credenziali ricevute dal proprietario della risorsa. Quando si effettua la richiesta, il client esegue l'autenticazione con il server di autorizzazione. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) Il server di autorizzazione autentica il client e convalida le credenziali del proprietario della risorsa e, se valido, emette un token di accesso. |  |

Di seguito è `Provider.GrantResourceOwnerCredentials`riportata l'implementazione di esempio per :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Il codice precedente ha lo scopo di spiegare questa sezione dell'esercitazione e non deve essere usato nelle app sicure o di produzione. Non controlla le credenziali dei proprietari delle risorse. Si presuppone che ogni credenziale sia valida e crea una nuova identità per esso. La nuova identità verrà utilizzata per generare il token di accesso e il token di aggiornamento. Si prega di sostituire il codice con il proprio codice di gestione dell'account sicuro.


### <a name="client-credentials-grant"></a>Concessione credenziali client

Fare riferimento alla sezione OAuth 2 [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) di IETF.

 Il flusso [di concessione delle credenziali client](http://tools.ietf.org/html/rfc6749#section-4.4) illustrato nella Figura 6 è il flusso e il mapping che segue il middleware OWIN OAuth.

| Passaggi del flusso dalla sezione Concessione credenziali client | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) Il client esegue l'autenticazione con il server di autorizzazione e richiede un token di accesso dall'endpoint del token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) Il server di autorizzazione autentica il client e, se valido, emette un token di accesso. |  |

Di seguito è `Provider.GrantClientCredentials`riportata l'implementazione di esempio per :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Il codice precedente ha lo scopo di spiegare questa sezione dell'esercitazione e non deve essere usato nelle app sicure o di produzione. Sostituire il codice con il proprio codice di gestione client sicuro.


### <a name="refresh-token"></a>Aggiornare un token

Fare riferimento alla sezione [Del token](http://tools.ietf.org/html/rfc6749#section-1.5) di aggiornamento OAuth 2 di IETF.

 Il flusso [di token](http://tools.ietf.org/html/rfc6749#section-1.5) di aggiornamento illustrato nella Figura 2 è il flusso e il mapping che segue il middleware OAuth OWIN.

| Passaggi del flusso dalla sezione Concessione credenziali client | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (G) Il client richiede un nuovo token di accesso eseguendo l'autenticazione con il server di autorizzazione e presentando il token di aggiornamento. I requisiti di autenticazione client si basano sul tipo di client e sui criteri del server di autorizzazione. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) Il server di autorizzazione autentica il client e convalida il token di aggiornamento e, se valido, emette un nuovo token di accesso (e, facoltativamente, un nuovo token di aggiornamento). |  |

Di seguito è `Provider.GrantRefreshToken`riportata l'implementazione di esempio per :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Creare un server di risorse protetto dal token di accessoCreate a Resource Server which is protected by Access Token

Creare un progetto di app Web vuoto e installare i pacchetti seguenti nel progetto:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Creare una classe di avvio e configurare l'autenticazione e l'API Web.Create a startup class and configure authentication and Web API. Nell'esempio di download, vedere *AuthorizationServer/ResourceServer/Startup.cs.*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Nell'esempio di download, vedere *AuthorizationServer.\_*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Nell'esempio di download, vedere Il server di autorizzazione del server delle *risorse, l'avvio dell'app.\_*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`metodo consente CORS per tutti i domini.
- `UseOAuthBearerAuthentication`metodo abilita il middleware di autenticazione del token di connessione OAuth che riceverà e convaliderà il token di connessione dall'intestazione di autorizzazione nella richiesta.
- `Config.SuppressDefaultHostAuthenticaiton`elimina l'entità autenticata dell'host predefinita dall'app, pertanto tutte le richieste saranno anonime dopo questa chiamata.
- `HostAuthenticationFilter`abilita l'autenticazione solo per il tipo di autenticazione specificato. In questo caso, è il tipo di autenticazione al portatore.

Per dimostrare l'identità autenticata, viene creato un ApiController per restituire le attestazioni dell'utente corrente.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Se il server di autorizzazione e il server delle risorse non si trovano nello stesso computer, il middleware OAuth utilizzerà le diverse chiavi del computer per crittografare e decrittografare il token di accesso al portatore. Per condividere la stessa chiave privata tra entrambi `machinekey` i progetti, aggiungiamo la stessa impostazione in entrambi i file *web.config.*

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Creazione di client OAuth 2.0

 Usiamo il pacchetto [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet per semplificare il codice client.

### <a name="authorization-code-grant-client"></a>Codice di autorizzazione Concedere il cliente

 Questo client è un'applicazione MVC. Verrà attivato un flusso di concessione del codice di autorizzazione per ottenere il token di accesso dal back-end. Ha una singola pagina come mostrato di seguito:

![](owin-oauth-20-authorization-server/_static/image3.png)

- Il pulsante **Autorizza** reindirizzerà il browser al server di autorizzazione per notificare al proprietario della risorsa di concedere l'accesso a questo client.
- Il pulsante **Aggiorna** otterrà un nuovo token di accesso e un token di aggiornamento usando il token di aggiornamento corrente.
- Il pulsante **Access Protected Resource API** chiamerà il server delle risorse per ottenere i dati sulle attestazioni dell'utente corrente e visualizzarli nella pagina.

Di seguito è riportato il codice di esempio `HomeController` del client.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`richiede SSL per impostazione predefinita. Poiché la demo utilizza HTTP, è necessario aggiungere le seguenti impostazioni nel file di configurazione:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Sicurezza: non disabilitare mai SSL in un'app di produzione. Le credenziali di accesso vengono ora inviate in testo non crittografato attraverso la rete. Il codice precedente è solo per il debug di esempio locale e l'esplorazione.


### <a name="implicit-grant-client"></a>Cliente grant implicito

Questo client utilizza JavaScript per:

1. Aprire una nuova finestra e reindirizzare all'endpoint di autorizzazione del server di autorizzazione.
2. Ottenere il token di accesso dai frammenti di URL quando viene reindirizzato.

L'immagine seguente mostra questo processo:The following image shows this process:

![](owin-oauth-20-authorization-server/_static/image4.png)

Il client deve avere due pagine: una per la home page e l'altra per la richiamata. Di seguito è riportato il codice JavaScript di esempio trovato nel file *Index.cshtml:Here* is the sample JavaScript code found in the Index.cshtml file:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Di seguito è riportato il codice di gestione dei callback nel file *SignIn.cshtml:Here* is the callback handling code in SignIn.cshtml file:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Una procedura consigliata consiste nello spostare il codice JavaScript in un file esterno e non incorporarlo con il markup Razor. Per mantenere questo esempio semplice, sono stati combinati.


### <a name="resource-owner-password-credentials-grant-client"></a>Credenziali password proprietario risorsa Concedere conferma client

Usiamo un'app console per disfare questo client. Il codice è il seguente:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Credenziali client Concedi client

Analogamente alla concessione delle credenziali della password del proprietario della risorsa, ecco il codice dell'app console:Similar to the Resource Owner Password Credentials Grant, here is console app code:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
