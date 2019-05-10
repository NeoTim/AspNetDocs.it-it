---
uid: signalr/overview/security/introduction-to-security
title: Introduzione alla sicurezza di SignalR | Microsoft Docs
author: bradygaster
description: Vengono descritti i problemi di sicurezza che è necessario considerare quando si sviluppa un'applicazione di SignalR.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65113659"
---
# <a name="introduction-to-signalr-security"></a>Introduzione alla sicurezza di SignalR

dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive i problemi di sicurezza che è necessario considerare quando si sviluppa un'applicazione di SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
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
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Concetti relativi alla sicurezza di SignalR](#concepts)

    - [Autenticazione e autorizzazione](#authentication)
    - [Token di connessione](#connectiontoken)
    - [La nuova partecipazione a gruppi durante la riconnessione](#rejoingroup)
- [Come SignalR impedisce Cross-Site Request Forgery](#csrf)
- [Raccomandazioni sulla sicurezza di SignalR](#recommendations)

    - [Protocollo SSL (Socket Layers) sicuro](#ssl)
    - [Non usare i gruppi come meccanismo di sicurezza](#groupsecurity)
    - [Gestione dell'input dai client in modo sicuro](#input)
    - [Riconciliazione di una modifica nello stato di utente con una connessione attiva](#reconcile)
    - [File generati automaticamente proxy di JavaScript](#autogen)
    - [Eccezioni](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concetti relativi alla sicurezza di SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticazione e autorizzazione

SignalR non fornisce alcuna funzionalità per l'autenticazione degli utenti. Al contrario, si integrano le funzionalità di SignalR nella struttura di autenticazione esistenti per un'applicazione. Si autenticano gli utenti normalmente, come sarebbe nell'applicazione e funzionano con i risultati dell'autenticazione in SignalR il codice. Ad esempio, si autenticano gli utenti con autenticazione basata su form ASP.NET e quindi nell'hub, imporre quali utenti o ruoli autorizzati a chiamare un metodo. Nell'hub, è anche possibile passare le informazioni di autenticazione, ad esempio nome utente o se un utente appartiene a un ruolo, al client.

SignalR fornisce il [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attributo per specificare quali utenti hanno accesso a un hub o un metodo. Si applica l'attributo Authorize a un hub o metodi particolari in un hub. Senza l'attributo Authorize, tutti i metodi pubblici in hub sono disponibili per un client connessa all'hub. Per altre informazioni sugli hub, vedere [l'autenticazione e autorizzazione per SignalR Hubs](hub-authorization.md).

Si applica il `Authorize` hub, ma le connessioni non persistenti dell'attributo. Per applicare le regole di autorizzazione quando si usa un' `PersistentConnection` è necessario eseguire l'override di `AuthorizeRequest` (metodo). Per altre informazioni sulle connessioni permanenti, vedere [autenticazione e autorizzazione per le connessioni persistenti di SignalR](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token di connessione

SignalR riduce il rischio di esecuzione di comandi dannosi per la convalida dell'identità del mittente. Per ogni richiesta, il client e server passare un token di connessione che contiene l'id di connessione e il nome utente per gli utenti autenticati. L'id di connessione identifica in modo univoco ogni client connessi. Il server in modo casuale genera l'id di connessione quando viene creata una nuova connessione e tale id viene mantenuto per la durata della connessione. Il meccanismo di autenticazione per l'applicazione web fornisce il nome utente. SignalR utilizza una firma digitale e crittografia per proteggere il token di connessione.

![](introduction-to-security/_static/image2.png)

Per ogni richiesta, il server convalida il contenuto del token per assicurarsi che la richiesta provenga da parte dell'utente specificato. Il nome utente deve corrispondere all'id di connessione. Convalidando l'id di connessione e il nome utente, SignalR impedisce a un utente malintenzionato di rappresentare facilmente un altro utente. Se il server non è possibile convalidare il token di connessione, la richiesta ha esito negativo.

![](introduction-to-security/_static/image4.png)

Poiché l'id di connessione fa parte del processo di verifica, devi non rivelare l'id di connessione di un utente ad altri utenti o archiviare il valore sul client, ad esempio in un cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Token di connessione e altri tipi di token

I token di connessione in alcuni casi sono contrassegnati dagli strumenti di sicurezza in quanto vengono visualizzati sia i token di sessione o i token di autenticazione, che pone un problema se esposte.

Token di connessione di SignalR non è un token di autenticazione. Viene utilizzato per confermare che l'utente che effettua la richiesta corrisponde a quello che ha creato la connessione. Il token di connessione è necessario perché ASP.NET SignalR consente le connessioni per spostarsi tra i server. Il token associa la connessione a un determinato utente ma non l'asserzione l'identità dell'utente che effettua la richiesta. Per una richiesta SignalR essere propriamente autenticati, deve avere un altro token che asserisce l'identità dell'utente, ad esempio un cookie o token di connessione. Tuttavia, la connessione del token stesso rende alcuna attestazione che la richiesta è stata effettuata dall'utente, solo che l'ID di connessione contenuta all'interno del token non è associato a tale utente.

Poiché il token di connessione non fornita alcuna attestazione di autenticazione propria, non è considerato una "sessione" o "autenticazione" token. Accettando token connessione di un utente specificato e la riesecuzione di una richiesta venga autenticata come un altro utente (o una richiesta non autenticata) avrà esito negativo, perché l'identità dell'utente della richiesta e l'identità archiviate nel token non corrispondenti.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>La nuova partecipazione a gruppi durante la riconnessione

Per impostazione predefinita, l'applicazione di SignalR verrà nuovamente assegnare automaticamente un utente ai gruppi appropriati durante la riconnessione dopo un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Durante la riconnessione, il client passa un token di gruppo che include l'id di connessione e i gruppi assegnati. Il token gruppo firma digitale viene firmato e crittografato. Il client mantiene lo stesso id di connessione dopo una riconnessione. Pertanto, l'id di connessione passata dal client riconnesso deve corrispondere all'id di connessione precedente usato dal client. Questa verifica impedisce il passaggio di richieste di partecipazione a gruppi non autorizzati durante la riconnessione di un utente malintenzionato.

Tuttavia, è importante tenere presente che il token di gruppo non ha scadenza. Se un utente appartiene a un gruppo in precedenza, ma è stato escluso da questo gruppo, tale utente sia in grado di simulare un token di gruppo che includa il gruppo non consentito. Se è necessario gestire in modo sicuro gli utenti che appartengono a quali gruppi, è necessario archiviare i dati nel server, ad esempio in un database. Quindi, aggiungere logica all'applicazione che verifica sul server se un utente appartiene a un gruppo. Per un esempio di verifica dell'appartenenza al gruppo, vedere [utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

Automaticamente la nuova partecipazione a gruppi si applica solo quando una connessione viene ristabilita la connessione dopo un'interruzione temporanea. Se un utente si disconnette dal passaggio a un'altra applicazione o del riavvio dell'applicazione, l'applicazione deve gestire come aggiungere l'utente ai gruppi corretti. Per altre informazioni, vedere [utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Come SignalR impedisce Cross-Site Request Forgery

Cross-Site richiesta intersito falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso. SignalR impedisce CSRF, rendendo estremamente improbabile per un sito dannoso creare una richiesta valida per l'applicazione di SignalR.

### <a name="description-of-csrf-attack"></a>Descrizione dell'attacco CSRF

Di seguito è riportato un esempio di un attacco CSRF:

1. Un utente accede a www.example.com, tramite autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza la disconnessione, l'utente visita un sito web dannoso. Questo sito dannoso contiene il form HTML seguente:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Questa è la parte "intersito" di CSRF.
4. L'utente fa clic sul pulsante Invia. Il visualizzatore include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita nel server di example.com con contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che un utente autenticato può eseguire.

Anche se in questo esempio richiede all'utente di fare clic sul pulsante del form, la pagina dannosa potrebbe facilmente eseguire uno script che invia una richiesta AJAX all'applicazione di SignalR. Inoltre, con SSL non impedire un attacco CSRF, poiché il sito dannoso può inviare una richiesta di "https://".

In genere, gli attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, perché i browser inviano tutti i cookie pertinenti al sito web di destinazione. Attacchi CSRF, tuttavia, non sono limitati a sfruttare i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali fino a quando non termina la sessione.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigazioni CSRF usati da SignalR

SignalR esegue i passaggi seguenti per impedire a un sito dannoso di creazione di richieste valide per l'applicazione. SignalR è necessario eseguire queste operazioni per impostazione predefinita, non è necessario intraprendere alcuna azione nel codice.

- **Disabilitare le richieste tra domini** SignalR disabilita le richieste tra domini per impedire agli utenti di chiamare un endpoint SignalR dal dominio esterno. SignalR prende in considerazione qualsiasi richiesta proveniente da un dominio esterno non è valido e blocca la richiesta. È consigliabile mantenere questo comportamento predefinito; in caso contrario, un sito dannoso potrebbe indurre gli utenti a inviare comandi al sito. Se è necessario usare richieste tra domini, vedere [come stabilire una connessione cross-domain](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passare il token di connessione nella stringa di query, cookie non** SignalR passa il token di connessione come un valore di stringa di query, anziché sotto forma di cookie. L'archiviazione dei token di connessione in un cookie non è sicuro perché il browser inavvertitamente può inoltrare il token di connessione quando viene rilevato malware. Inoltre, passando il token di connessione nella stringa di query impedisce il token di connessione renda persistente oltre la connessione corrente. Pertanto, un utente malintenzionato non può effettuare una richiesta con credenziali di autenticazione di un altro utente.
- **Verificare i token di connessione** come descritto nel [il token di connessione](#connectiontoken) sezione, il server sa quale id di connessione è associato a ciascun utente autenticato. Il server non elabora qualsiasi richiesta proveniente da un id di connessione che non corrisponde al nome utente. È improbabile che un utente malintenzionato è stato possibile ipotizzare una richiesta valida perché l'utente malintenzionato sarebbe dover conoscere il nome utente e l'id generati in modo casuale connessione corrente. Tale id di connessione non è più valido, non appena la connessione viene terminata. Gli utenti anonimi non dovrebbero avere accesso a informazioni riservate.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Raccomandazioni sulla sicurezza di SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocollo SSL (Socket Layers) sicuro

Il protocollo SSL Usa la crittografia per proteggere il trasporto dei dati tra un client e server. Se l'applicazione di SignalR trasmette le informazioni riservate tra il client e server, utilizzare SSL per il trasporto. Per altre informazioni sull'impostazione di SSL, vedere [come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Non usare i gruppi come meccanismo di sicurezza

I gruppi sono un modo pratico per la raccolta di utenti correlati, ma non sono un meccanismo protetto per limitare l'accesso a informazioni riservate. Ciò è particolarmente vero quando gli utenti possono automaticamente accedere nuovamente gruppi durante una riconnessione. Al contrario, prendere in considerazione l'aggiunta di utenti con privilegi a un ruolo e limitare l'accesso a un metodo dell'hub per solo i membri del ruolo in questione. Per un esempio di limitazione dell'accesso in base a un ruolo, vedere [l'autenticazione e autorizzazione per SignalR Hubs](hub-authorization.md). Per un esempio di verifica dell'accesso utente ai gruppi durante la riconnessione, vedere [utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gestione dell'input dai client in modo sicuro

Per garantire che un utente malintenzionato non invia script ad altri utenti, è necessario codificare tutti gli input dai client destinato a trasmettere ad altri client. Si consiglia di codificare i messaggi sul client ricevente invece che nel server, perché l'applicazione di SignalR può avere molti tipi diversi di client. Pertanto, la codifica HTML funziona per un client web, ma non per altri tipi di client. Ad esempio, un metodo di client web per visualizzare un messaggio di chat in modo sicuro gestirà il nome utente e il messaggio chiamando il `html()` (funzione).

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Riconciliazione di una modifica nello stato di utente con una connessione attiva

Se lo stato di autenticazione dell'utente viene modificato mentre è presente una connessione attiva, l'utente riceverà un errore che indica, "durante una connessione SignalR attiva non è possibile modificare l'identità dell'utente". In tal caso, l'applicazione deve connettersi nuovamente al server per assicurarsi che il nome utente e l'id di connessione siano coordinati. Ad esempio, se l'applicazione consente all'utente di disconnettersi mentre è presente una connessione attiva, il nome utente per la connessione non corrisponderanno più il nome che viene passato per la richiesta successiva. Si verrà desidera interrompere la connessione prima che l'utente si disconnette e quindi lo riavvia.

Tuttavia, è importante notare che la maggior parte delle applicazioni non saranno necessario arrestare e avviare la connessione manualmente. Se l'applicazione reindirizza gli utenti a una pagina separata dopo l'accesso, ad esempio il comportamento predefinito in un'applicazione Web Form o MVC o aggiorna la pagina corrente dopo la disconnessione, la connessione attiva viene interrotta automaticamente e non esiste richiedere altre operazioni.

Nell'esempio seguente viene illustrato come arrestare e avviare una connessione quando è stato modificato lo stato dell'utente.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

In alternativa, lo stato di autenticazione dell'utente può cambiare se il sito utilizza la scadenza con autenticazione basata su form e senza alcuna attività per mantenere il cookie di autenticazione valido. In tal caso, l'utente verrà disconnesso e il nome utente non corrisponderanno più il nome utente nel token di connessione. È possibile risolvere questo problema, aggiungere alcuni script che periodicamente richiede una risorsa nel server web per mantenere il cookie di autenticazione valido. Nell'esempio seguente viene illustrato come richiedere una risorsa ogni 30 minuti.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>File generati automaticamente proxy di JavaScript

Se non vuoi includere tutti gli hub e i metodi nel file proxy JavaScript per ogni utente, è possibile disabilitare la generazione automatica del file. È possibile scegliere questa opzione se sono presenti più hub e i metodi, ma non tutti gli utenti da tenere presenti tutti i metodi. Disabilitare la generazione automatica impostando **EnableJavaScriptProxies** al **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Per altre informazioni sui file proxy JavaScript, vedere [proxy generato e che cosa fa per te](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Eccezioni

È consigliabile evitare il passaggio di oggetti eccezione ai client in quanto gli oggetti potrebbero contenere informazioni riservate per i client. In alternativa, è possibile chiamare un metodo sul client che viene visualizzato il messaggio di errore pertinenti.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
