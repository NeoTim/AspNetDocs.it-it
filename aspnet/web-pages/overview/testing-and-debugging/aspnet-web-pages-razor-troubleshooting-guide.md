---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) Troubleshooting Guide | Microsoft Docs
author: Rick-Anderson
description: Questo articolo vengono descritti i problemi che possono verificarsi quando si lavora con ASP.NET Web Pages (Razor) e alcune soluzioni suggerite. Versioni del software ASP.NET Web Pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65128567"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)

da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo vengono descritti i problemi che possono verificarsi quando si lavora con ASP.NET Web Pages (Razor) e alcune soluzioni suggerite.
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e le pagine Web ASP.NET 1.0.

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Problemi con l'esecuzione di pagine](#Issues_Running_.cshtml_Pages)
- [Problemi con il codice Razor](#IssuesWithRazorCode)
- [Problemi relativi alla sicurezza e appartenenza](#membership)
- [Problemi con l'invio di posta elettronica](#email)
- [Risorse aggiuntive](#AdditionalResources)

Per domande generali, vedere [ASP.NET Web Pages (Razor) domande frequenti su](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemi con l'esecuzione di pagine

Un'ampia gamma di problemi possibile impedire *. cshtml* e *vbhtml* pagine funzioni correttamente. In questa sezione elenca i messaggi di errore comuni e cause possibili.

### <a name="http-error-403---forbidden-access-is-denied"></a>Errore HTTP 403 - accesso negato: Accesso negato

*Non è autorizzato a visualizzare la directory o la pagina utilizzando le credenziali fornite.*

Questo errore può verificarsi se il server non è in esecuzione la versione corretta di .NET Framework. Assicurarsi che il computer che esegue il server (locale o remota) abbia almeno .NET Framework 4. Assicurarsi anche che l'applicazione stessa è configurato per eseguire la versione corretta.

Se si riscontra questo problema in locale mentre si lavora in WebMatrix, fare clic sui **Site** dell'area di lavoro e nella visualizzazione albero fare clic su **impostazioni**. Nel **Seleziona versione di .NET Framework** , selezionare dalla lista **.NET 4 (Integrated)**. Se questa versione è già impostata, provare a eseguire WebMatrix come amministratore.

Assicurarsi che sia la radice del sito Web ad almeno uno *cshtml* file in essa contenuti.

Se viene visualizzato questo errore quando il server web sia in un server remoto, contattare l'amministratore del server. Assicurarsi che il server di .NET Framework 4 o versione successiva installato. Assicurarsi anche che l'applicazione sia in esecuzione in un pool di applicazioni configurato per usare tale versione di.NET Framework.

Se si dispone di controllo del server, assicurarsi che è in esecuzione la versione corretta di .NET Framework. È possibile anche provare a ripristinare l'installazione eseguendo il `aspnet_regiis -iru` comando. (Ad esempio, se si installa IIS dopo l'installazione di .NET Framework, IIS sarà non essere configurato correttamente per eseguire le pagine ASP.NET.) Per altre informazioni, vedere [strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Errore HTTP 403.14 - accesso negato

*Il server Web è configurato per non visualizzare il contenuto di questa directory.*

Questo errore può verificarsi se si richiede una risorsa protetta (come il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).

### <a name="http-error-40417---not-found"></a>Errore HTTP 404.17 - non trovato

*Il contenuto richiesto sembra essere script e non verrà servito dal gestore di file statici.*

Questo errore può verificarsi se il server non è configurato correttamente per l'uso di .NET Framework 4 o versione successiva e pertanto non riconosce il codice in `@{ }` blocchi. Vedere la descrizione in precedenza per *HTTP Errore 403 - accesso negato: Accesso negato*.

### <a name="http-error-4047---not-found"></a>Errore HTTP 404.7 - non trovato

*Il modulo filtro delle richieste sono configurata per rifiutare l'estensione di file*

Questo errore può verificarsi se *. cshtml* oppure *vbhtml* estensioni sono state bloccate in modo esplicito nel server. Un sintomo di questo problema è che funzionano URL quando non includono l'estensione, ma gli URL che includono *. cshtml* oppure *vbhtml* non funzionano. Una soluzione possibile consiste nell'abilitare nuovamente le estensioni del sito *Web. config* file. Nell'esempio seguente viene illustrato come abilitare il *cshtml* estensione.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Errore HTTP 404.8 - non trovato

*Il modulo filtro delle richieste sono configurata per rifiutare un percorso nell'URL che contiene una sezione hiddenSegment.*

Questo errore può verificarsi se si richiede una risorsa protetta (come il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Questo tipo di pagina non è servito (errore Server nell'applicazione '/')

Vedere la descrizione in precedenza per HTTP 404.17 di errore.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemi con il codice Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Il nome '*classe*' non esiste nel contesto corrente

Un motivo viene visualizzato questo errore è spesso che `class` riferimenti a un file di supporto, ma l'helper non è installato. Ad esempio, se si prova a usare un file di supporto, ma se non è stato installato il pacchetto da NuGet, verrà visualizzato questo errore. Usa la raccolta di WebMatrix per trovare e installare il supporto.

Se l'helper è installato, ma la pagina ancora non lo riconosce, provare ad aggiungere aggiungere un `using` istruzione per il codice. Nel `using` istruzione, lo spazio dei nomi che include il supporto di riferimento. Ad esempio, gli helper di base che sono nel pacchetto di ASP.NET Web Helpers sono nel `System.Web.Helpers` dello spazio dei nomi. Nella parte superiore della pagina in cui si desidera utilizzare l'helper, aggiungere questa riga:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemi relativi alla sicurezza e appartenenza

Se si usa il sistema di sicurezza incorporati (membership) in ASP.NET Web Pages (Razor), si potrebbero verificarsi i problemi seguenti.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Per chiamare questo metodo, la proprietà "Membership.Provider" deve essere un'istanza di "ExtendedMembershipProvider"

Questo errore può indicare che nessun `AspNetSqlMembershipProvider` classe è configurata. (Un sintomo è che il sito funziona correttamente in locale, ma genera questo errore quando lo si pubblica per il server del provider di hosting). Una soluzione al problema consiste nell'abilitare in modo esplicito l'appartenenza semplice aggiungendo il codice seguente del sito *Web. config* file:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemi con l'invio di posta elettronica

Problemi con l'invio di posta elettronica possono essere difficile eseguire il debug. Un problema iniziale è possibile che non è possibile connettersi al server SMTP. Se la connessione ha esito positivo, ASP.NET invia il messaggio al server SMTP. Tuttavia, si possono verificare problemi con il messaggio stesso che impedisce l'invio al server SMTP.

Se l'applicazione non correttamente Invia messaggio di posta elettronica, procedere come segue:

- Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`. Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a quel punto possibile `localhost`. Questa situazione si verifica perché dopo aver pubblicato e il sito è in esecuzione nel server del provider, il server SMTP potrebbe essere locale dal punto di vista dell'applicazione. Questa modifica nei nomi di server potrebbe indicare che è necessario modificare il nome del server SMTP come parte del processo di pubblicazione.
- Il numero di porta in genere è 25. Tuttavia, alcuni provider richiedono di usare la porta 587 o alcune porte. Contattare il proprietario del server SMTP del numero di porta si aspettano da usare.
- Assicurarsi di usare le credenziali corrette. Se il sito è stata pubblicata in un provider di hosting, usare le credenziali che il provider ha segnalato in modo specifico sono per la posta elettronica. Queste credenziali possono essere diverse da quelle usate per la pubblicazione.
- In alcuni casi non occorre credenziali affatto. Se si invia tramite posta elettronica usando l'account personale, il provider di posta elettronica sappia già le proprie credenziali. Dopo aver pubblicato, è necessario usare credenziali diverse da quelle quando si testa nel computer locale.
- Se il provider di posta elettronica utilizza la crittografia, impostare `WebMail.EnableSsl` a `true`.

Se si verifica un errore l'invio di posta elettronica, si potrebbe visualizzare un messaggio di errore ASP.NET standard, che presenta un aspetto simile al seguente:

![Messaggio di errore ASP.NET quando si è verificato un problema con la posta elettronica](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

È anche possibile eseguire il debug dei problemi con l'invio di posta elettronica utilizzando un `try-catch` blocco, come nell'esempio seguente. Quando si usa un `try-catch` blocco, ASP.NET non visualizza messaggi di errore standard. In alternativa, è possibile acquisire l'errore nel `catch` parte del blocco.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Sostituire i valori appropriati per `your-SMTP-server-name`e così via. Di seguito alcuni dei messaggi di errore che potrebbe essere visualizzato in questo modo:

- *Errore durante l'invio di posta elettronica.*

    -oppure-

    *Un tentativo di connessione non è riuscito perché la parte connessa non ha risposto correttamente dopo un periodo di tempo o stabilire una connessione non è riuscita perché l'host connesso non ha risposto*

    Questo errore indica generalmente che l'applicazione non è riuscito a connettersi al server SMTP. Controllare il nome del server e il numero di porta.
- *Cassetta postale non disponibile. È stata la risposta del server: 5.1.0 &lt; someuser@invaliddomain &gt; mittente rifiutata: dominio mittente non valido*

    Questo messaggio può indicare che il `From` indirizzo non è corretto o mancante.
- *La stringa specificata non è nel formato richiesto per un indirizzo di posta elettronica.*

    Questo errore potrebbe indicare che il valore della `To` o `From` proprietà non sono riconosciute come indirizzi di posta elettronica. (ASP.NET non è possibile verificare che l'indirizzo di posta elettronica sia valido, ma solo la 's nel formato corretto, ad esempio *name@domain.com*.)

> [!NOTE]
> Rimuovere il markup che viene visualizzato l'errore (`@errorMessage`) prima di pubblicare la pagina in un sito live. Non è una buona idea per consentire agli utenti di visualizzare i messaggi di errore che si ottiene da un server.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Domande frequenti sulle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[ASP.NET Web Pages e WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) forum nel sito Web ASP.NET
