---
uid: signalr/overview/security/introduction-to-security
title: Introduzione alla sicurezza SignalR Documenti Microsoft
author: bradygaster
description: Descrive i problemi di sicurezza da considerare quando si sviluppa un'applicazione SignalR.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676195"
---
# <a name="introduction-to-signalr-security"></a>Introduzione alla sicurezza di SignalR

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo articolo vengono descritti i problemi di sicurezza da considerare quando si sviluppa un'applicazione SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina. Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Concetti di sicurezza SignalR](#concepts)

    - [Autenticazione e autorizzazione](#authentication)
    - [Token di connessione](#connectiontoken)
    - [Ricongiungirsi ai gruppi durante la riconnessione](#rejoingroup)
- [Come SignalR impedisce la contraffazione delle richieste tra siti](#csrf)
- [Raccomandazioni di sicurezza SignalR](#recommendations)

    - [Protocollo SSL (Secure Socket Layers)](#ssl)
    - [Non utilizzare i gruppi come meccanismo di sicurezza](#groupsecurity)
    - [Gestione sicura dell'input dai client](#input)
    - [Riconciliare una modifica dello stato utente con una connessione attiva](#reconcile)
    - [File proxy JavaScript generati automaticamente](#autogen)
    - [Eccezioni](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concetti di sicurezza SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticazione e autorizzazione

SignalR non fornisce alcuna funzionalità per l'autenticazione degli utenti. Al contrario, si integrano le funzionalità SignalR nella struttura di autenticazione esistente per un'applicazione. Gli utenti vengono autenticati come di consueto nell'applicazione e si utilizzano i risultati dell'autenticazione nel codice SignalR. Ad esempio, è possibile autenticare gli utenti con ASP.NET l'autenticazione basata su form e quindi nell'hub imporre quali utenti o ruoli sono autorizzati a chiamare un metodo. Nell'hub è inoltre possibile passare al client le informazioni di autenticazione, ad esempio il nome utente o se un utente appartiene a un ruolo.

SignalR fornisce l'attributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) per specificare quali utenti hanno accesso a un hub o a un metodo. Si applica l'attributo Authorize a un hub o a metodi particolari in un hub. Senza l'attributo Authorize, tutti i metodi pubblici nell'hub sono disponibili per un client connesso all'hub. Per ulteriori informazioni sugli hub, vedere [Autenticazione e autorizzazione per hub SignalR](hub-authorization.md).

Si applica `Authorize` l'attributo agli hub, ma non alle connessioni permanenti. Per applicare le regole `PersistentConnection` di autorizzazione `AuthorizeRequest` quando si utilizza un è necessario eseguire l'override del metodo. Per ulteriori informazioni sulle connessioni permanenti, vedere [Autenticazione e autorizzazione per connessioni persistenti SignalR](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token di connessione

SignalR riduce il rischio di esecuzione di comandi dannosi convalidando l'identità del mittente. Per ogni richiesta, il client e il server passano un token di connessione che contiene l'ID di connessione e il nome utente per gli utenti autenticati. L'ID di connessione identifica in modo univoco ogni client connesso. Il server genera in modo casuale l'ID di connessione quando viene creata una nuova connessione e mantiene tale ID per la durata della connessione. Il meccanismo di autenticazione per l'applicazione web fornisce il nome utente. SignalR utilizza la crittografia e una firma digitale per proteggere il token di connessione.

![](introduction-to-security/_static/image2.png)

Per ogni richiesta, il server convalida il contenuto del token per garantire che la richiesta provenga dall'utente specificato. Il nome utente deve corrispondere all'ID di connessione. Convalidando sia l'ID di connessione che il nome utente, SignalR impedisce a un utente malintenzionato di impersonare facilmente un altro utente. Se il server non è in grado di convalidare il token di connessione, la richiesta ha esito negativo.

![](introduction-to-security/_static/image4.png)

Poiché l'ID di connessione fa parte del processo di verifica, non è consigliabile rivelare l'ID di connessione di un utente ad altri utenti o archiviare il valore nel client, ad esempio in un cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Token di connessione e altri tipi di tokenConnection tokens vs.

I token di connessione vengono occasionalmente contrassegnati dagli strumenti di sicurezza perché sembrano essere token di sessione o token di autenticazione, il che rappresenta un rischio se esposti.

Il token di connessione di SignalR non è un token di autenticazione. Viene utilizzato per confermare che l'utente che effettua la richiesta è la stessa che ha creato la connessione. Il token di connessione è necessario perché ASP.NET SignalR consente lo spostamento delle connessioni tra i server. Il token associa la connessione a un utente specifico ma non assiafferma l'identità dell'utente che effettua la richiesta. Affinché una richiesta SignalR venga autenticata correttamente, deve disporre di un altro token che asserisce l'identità dell'utente, ad esempio un cookie o un token di connessione. Tuttavia, il token di connessione stesso non fa alcuna affermazione che la richiesta è stata effettuata da tale utente, solo che l'ID di connessione contenuto all'interno del token è associato a tale utente.

Poiché il token di connessione non fornisce alcuna attestazione di autenticazione propria, non è considerato un token "sessione" o "autenticazione". Prendendo il token di connessione di un determinato utente e riproducendolo in una richiesta autenticata come un utente diverso (o una richiesta non autenticata) avrà esito negativo, perché l'identità utente della richiesta e l'identità archiviata nel token non corrisponderà.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Ricongiungirsi ai gruppi durante la riconnessione

Per impostazione predefinita, l'applicazione SignalR riassegnerà automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene interrotta e ristabilita prima del timeout della connessione. Quando si riconnette, il client passa un token di gruppo che include l'ID di connessione e i gruppi assegnati. Il token di gruppo è firmato digitalmente e crittografato. Il client mantiene lo stesso ID di connessione dopo una riconnessione; pertanto, l'ID di connessione passato dal client riconnesso deve corrispondere all'ID di connessione precedente utilizzato dal client. Questa verifica impedisce a un utente malintenzionato di passare le richieste di unirsi a gruppi non autorizzati durante la riconnessione.

Tuttavia, è importante notare che il token di gruppo non scade. Se un utente apparteneva a un gruppo in passato, ma è stato escluso da tale gruppo, tale utente potrebbe essere in grado di simulare un token di gruppo che include il gruppo non consentito. Se è necessario gestire in modo sicuro quali utenti appartengono a quali gruppi, è necessario archiviare i dati nel server, ad esempio in un database. Aggiungere quindi la logica all'applicazione che verifica sul server se un utente appartiene a un gruppo. Per un esempio di verifica dell'appartenenza ai gruppi, vedere [Utilizzo dei gruppi.](../guide-to-the-api/working-with-groups.md)

Il ricongiungito automaticamente i gruppi si applica solo quando una connessione viene riconnessa dopo un'interruzione temporanea. Se un utente si disconnette allontanandosi dall'applicazione o l'applicazione viene riavviata, l'applicazione deve gestire come aggiungere l'utente ai gruppi corretti. Per ulteriori informazioni, consultate Operazioni con i [gruppi.](../guide-to-the-api/working-with-groups.md)

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Come SignalR impedisce la contraffazione delle richieste tra siti

Cross-Site Request Forgery (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è attualmente connesso. SignalR impedisce CSRF rendendo estremamente improbabile per un sito dannoso creare una richiesta valida per l'applicazione SignalR.

### <a name="description-of-csrf-attack"></a>Descrizione dell'attacco CSRF

Ecco un esempio di attacco CSRF:

1. Un utente accede a www.example.com, utilizzando l'autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza disconnettersi, l'utente visita un sito web dannoso. Questo sito dannoso contiene il seguente modulo HTML:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Si noti che l'azione modulo invia al sito vulnerabile, non al sito dannoso. Questa è la parte "cross-site" di CSRF.
4. L'utente fa clic sul pulsante di invio. Il browser include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita sul server example.com con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione consentita a un utente autenticato.

Anche se questo esempio richiede all'utente di fare clic sul pulsante del modulo, la pagina dannosa potrebbe altrettanto facilmente eseguire uno script che invia una richiesta AJAX all'applicazione SignalR. Inoltre, l'utilizzo di SSL non impedisce un attacco CSRF, perché il sito dannoso può inviare una richiesta "https://".

In genere, gli attacchi CSRF sono possibili contro siti web che utilizzano i cookie per l'autenticazione, perché i browser inviano tutti i cookie rilevanti al sito web di destinazione. Tuttavia, gli attacchi CSRF non si limitano allo sfruttamento dei cookie. Ad esempio, anche l'autenticazione di base e digest sono vulnerabili. Dopo che un utente accede con l'autenticazione di base o digest, il browser invia automaticamente le credenziali fino al termine della sessione.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigazioni CSRF prese da SignalR

SignalR esegue la procedura seguente per impedire a un sito dannoso di creare richieste valide per l'applicazione. SignalR esegue questi passaggi per impostazione predefinita, non è necessario eseguire alcuna azione nel codice.

- **Disabilitare le richieste tra dominiDisable cross domain requests** SignalR disabilita le richieste tra domini per impedire agli utenti di chiamare un endpoint SignalR da un dominio esterno. SignalR considera qualsiasi richiesta da un dominio esterno non valida e blocca la richiesta. Si consiglia di mantenere questo comportamento predefinito; in caso contrario, un sito dannoso potrebbe indurre gli utenti a inviare comandi al sito. Se è necessario utilizzare richieste tra domini, vedere [Come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) domini .
- Passare il token di connessione nella stringa di **query, non nel cookiePass connection token in query string, not cookie** SignalR passa il token di connessione come valore della stringa di query, anziché come cookie. L'archiviazione del token di connessione in un cookie non è sicura perché il browser può inavvertitamente inoltrare il token di connessione quando viene rilevato codice dannoso. Inoltre, il passaggio del token di connessione nella stringa di query impedisce che il token di connessione persista oltre la connessione corrente. Pertanto, un utente malintenzionato non può effettuare una richiesta con le credenziali di autenticazione di un altro utente.
- **Verificare il token di connessioneVerify connection token** Come descritto nella sezione Token di [connessione,](#connectiontoken) il server sa quale ID di connessione è associato a ogni utente autenticato. Il server non elabora alcuna richiesta da un ID di connessione che non corrisponde al nome utente. È improbabile che un utente malintenzionato possa indovinare una richiesta valida perché l'utente malintenzionato dovrebbe conoscere il nome utente e l'ID di connessione generato casualmente corrente. Tale ID di connessione diventa non valido non appena la connessione viene terminata. Gli utenti anonimi non dovrebbero avere accesso a informazioni riservate.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Raccomandazioni di sicurezza SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocollo SSL (Secure Socket Layers)

Il protocollo SSL utilizza la crittografia per proteggere il trasporto dei dati tra un client e un server. Se l'applicazione SignalR trasmette informazioni riservate tra il client e il server, utilizzare SSL per il trasporto. Per ulteriori informazioni sulla configurazione di SSL, vedere [Come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Non utilizzare i gruppi come meccanismo di sicurezza

I gruppi sono un modo pratico per raccogliere gli utenti correlati, ma non sono un meccanismo sicuro per limitare l'accesso alle informazioni riservate. Ciò è particolarmente vero quando gli utenti possono riconnettersi automaticamente i gruppi durante una riconnessione. Considerare invece l'aggiunta di utenti con privilegi a un ruolo e la limitazione dell'accesso a un metodo hub solo ai membri di tale ruolo. Per un esempio di limitazione dell'accesso in base a un ruolo, vedere [Autenticazione e autorizzazione per hub SignalR](hub-authorization.md). Per un esempio di controllo dell'accesso degli utenti ai gruppi durante la riconnessione, vedere [Utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gestione sicura dell'input dai client

Per garantire che un utente malintenzionato non invii script ad altri utenti, è necessario codificare tutti gli input dai client destinati alla trasmissione ad altri client. È consigliabile codificare i messaggi sui client riceventi anziché sul server, perché l'applicazione SignalR può avere molti tipi diversi di client. Pertanto, la codifica HTML funziona per un client web, ma non per altri tipi di client. Ad esempio, un metodo del client Web per visualizzare un messaggio `html()` di chat gestirà in modo sicuro il nome utente e il messaggio chiamando la funzione.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Riconciliare una modifica dello stato utente con una connessione attiva

Se lo stato di autenticazione di un utente cambia mentre esiste una connessione attiva, l'utente riceverà un errore che indica: "L'identità dell'utente non può cambiare durante una connessione SignalR attiva". In tal caso, l'applicazione deve riconnettersi al server per assicurarsi che l'ID di connessione e il nome utente siano coordinati. Ad esempio, se l'applicazione consente all'utente di disconnettersi mentre esiste una connessione attiva, il nome utente per la connessione non corrisponderà più al nome che viene passato per la richiesta successiva. È consigliabile interrompere la connessione prima che l'utente si disconta e quindi riavviarla.

Tuttavia, è importante notare che la maggior parte delle applicazioni non sarà necessario arrestare e avviare manualmente la connessione. Se l'applicazione reindirizza gli utenti a una pagina separata dopo la disconnessione, ad esempio il comportamento predefinito in un'applicazione Web Form o MVC, o aggiorna la pagina corrente dopo la disconnessione, la connessione attiva viene disconnessa automaticamente e non richiede alcuna azione aggiuntiva.

Nell'esempio seguente viene illustrato come arrestare e avviare una connessione quando lo stato dell'utente è cambiato.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

In alternativa, lo stato di autenticazione dell'utente può cambiare se il sito utilizza la scadenza variabile con l'autenticazione basata su form e non vi è alcuna attività per mantenere valido il cookie di autenticazione. In tal caso, l'utente verrà disconnesso e il nome utente non corrisponderà più al nome utente nel token di connessione. È possibile risolvere questo problema aggiungendo uno script che richiede periodicamente una risorsa sul server web per mantenere valido il cookie di autenticazione. Nell'esempio seguente viene illustrato come richiedere una risorsa ogni 30 minuti.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>File proxy JavaScript generati automaticamente

Se non si desidera includere tutti gli hub e i metodi nel file proxy JavaScript per ogni utente, è possibile disabilitare la generazione automatica del file. È possibile scegliere questa opzione se si dispone di più hub e metodi, ma non si desidera che tutti gli utenti siano a conoscenza di tutti i metodi. Per disabilitare la generazione automatica, **impostare EnableJavaScriptProxies** su **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Per ulteriori informazioni sui file proxy JavaScript, consultate [Il proxy generato e le operazioni eseguite automaticamente.](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) <a id="exceptions"></a>

### <a name="exceptions"></a>Eccezioni

È consigliabile evitare di passare oggetti eccezione ai client perché gli oggetti possono esporre informazioni riservate ai client. Chiamare invece un metodo sul client che visualizza il messaggio di errore pertinente.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
