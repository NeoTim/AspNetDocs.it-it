---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'ASP.NET Distribuzione Web tramite Visual Studio: trasformazioni del file Web.config Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in App Service Web Apps o in un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676125"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET distribuzione Web tramite Visual Studio: trasformazioni del file Web.config

da parte [di Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in App Service Web Apps di Azure o in un provider di hosting di terze parti usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come automatizzare il processo di modifica del file *Web.config* quando lo si distribuisce in ambienti di destinazione diversi. La maggior parte delle applicazioni dispone di impostazioni nel file *Web.config* che devono essere diverse quando l'applicazione viene distribuita. Automatizzare il processo di apportare queste modifiche ti impedisce di doverle fare manualmente ogni volta che distribuisci, il che sarebbe noioso e soggetto a errori.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esercitazione, assicurarsi di controllare la pagina di [risoluzione dei problemi](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Trasformazioni di Web.config rispetto ai parametri di distribuzione Web

Esistono due modi per automatizzare il processo di modifica delle impostazioni del file *Web.config:* [trasformazioni Web.config](https://msdn.microsoft.com/library/dd465326.aspx) e [parametri di distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). Un file di trasformazione *Web.config* contiene markup XML che specifica come modificare il file *Web.config* quando viene distribuito. È possibile specificare modifiche diverse per configurazioni di compilazione specifiche e per profili di pubblicazione specifici. Le configurazioni di compilazione predefinite sono Debug e Release ed è possibile creare configurazioni di compilazione personalizzate. Un profilo di pubblicazione corrisponde in genere a un ambiente di destinazione. Per altre informazioni sui profili di pubblicazione, vedere l'esercitazione [Distribuzione in IIS come ambiente](deploying-to-iis.md) di test.

I parametri di distribuzione Web possono essere utilizzati per specificare molti tipi diversi di impostazioni che devono essere configurate durante la distribuzione, incluse le impostazioni disponibili nei file *Web.config.* Se utilizzati per specificare le modifiche al file *Web.config,* i parametri di distribuzione Web sono più complessi da configurare, ma sono utili quando non si conosce il valore da impostare fino alla distribuzione. Ad esempio, in un ambiente aziendale, è possibile creare un pacchetto di *distribuzione* e assegnarlo a una persona del reparto IT da installare nell'ambiente di produzione e tale persona deve essere in grado di immettere stringhe di connessione o password non notate.

Per lo scenario illustrato in questa serie di esercitazioni, è necessario conoscere in anticipo tutte le operazione da eseguire per il file *Web.config,* pertanto non è necessario utilizzare i parametri di distribuzione Web. Verranno configurate alcune trasformazioni che variano a seconda della configurazione di compilazione utilizzata e alcune che variano a seconda del profilo di pubblicazione utilizzato.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Specifica delle impostazioni di Web.config in AzureSpecifying Web.config settings in Azure

Se le impostazioni del file *Web.config* che `<connectionStrings>` si `<appSettings>` desidera modificare si trovano nell'elemento o e se si esegue la distribuzione in app Web nel servizio app di Azure, è disponibile un'altra opzione per l'automazione delle modifiche durante la distribuzione. È possibile immettere le impostazioni che si vuole avere effetto in Azure nella scheda **Configura** della pagina del portale di gestione per l'app Web (scorrere verso il basso fino alle sezioni **impostazioni dell'app** e stringhe di **connessione).** Quando si distribuisce il progetto, Azure applica automaticamente le modifiche. Per ulteriori informazioni, vedere Siti Web di [Windows Azure: funzionamento](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)delle stringhe di applicazione e delle stringhe di connessione .

## <a name="default-transformation-files"></a>File di trasformazione predefiniti

In **Esplora soluzioni**espandere *Web.config* per visualizzare i file di trasformazione *Web.Debug.config* e *Web.Release.config* creati per impostazione predefinita per le due configurazioni di compilazione predefinite.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

È possibile creare file di trasformazione per configurazioni di compilazione personalizzate facendo clic con il pulsante destro del mouse sul file Web.config e scegliendo **Aggiungi trasformazioni** di configurazione dal menu di scelta rapida. Per questa esercitazione non è necessario eseguire questa operazione e l'opzione di menu è disabilitata, perché non sono state create configurazioni di compilazione personalizzate.

Successivamente verranno creati altri tre file di trasformazione, uno per i profili di pubblicazione di test, gestione temporanea e produzione. Un esempio tipico di un'impostazione che è possibile gestire in un file di trasformazione del profilo di pubblicazione perché dipende dall'ambiente di destinazione è un endpoint WCF diverso per il test e la produzione. I file di trasformazione dei profili di pubblicazione verranno creati nelle esercitazioni successive dopo aver creato i profili di pubblicazione.

## <a name="disable-debug-mode"></a>Disattivare la modalità di debug

Un esempio di un'impostazione che dipende dalla `debug` configurazione di compilazione anziché dall'ambiente di destinazione è l'attributo. Per una build di rilascio, in genere si desidera che il debug sia disabilitato indipendentemente dall'ambiente in cui si esegue la distribuzione. Pertanto, per impostazione predefinita i modelli di progetto di Visual `debug` Studio `compilation` creano file di trasformazione *Web.Release.config* con codice che rimuove l'attributo dall'elemento. Di seguito è riportato il valore predefinito *Web.Release.config*: oltre a qualche `compilation` codice di `debug` trasformazione di esempio che viene commentato, include il codice nell'elemento che rimuove l'attributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

L'attributo `xdt:Transform="RemoveAttributes(debug)"` specifica che `debug` si desidera rimuovere `system.web/compilation` l'attributo dall'elemento nel file *Web.config* distribuito. Questa operazione verrà eseguita ogni volta che si distribuisce una build di rilascio.

## <a name="limit-error-log-access-to-administrators"></a>Limitare l'accesso al log degli errori agli amministratori

Se si verifica un errore durante l'esecuzione dell'applicazione, l'applicazione visualizza una pagina di errore generico al posto della pagina di errore generata dal sistema e utilizza il [pacchetto NuGet di Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) per la registrazione degli errori e la segnalazione. L'elemento `customErrors` nel file *Web.config* dell'applicazione specifica la pagina di errore:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Per visualizzare la pagina di `mode` errore, `customErrors` modificare temporaneamente l'attributo dell'elemento da "RemoteOnly" a "On" ed eseguire l'applicazione da Visual Studio. Causa di un errore richiedendo un URL non valido, ad esempio *Studentsxxx.aspx*. Invece di una pagina di errore "Impossibile trovare la risorsa" generata da IIS, viene visualizzata la pagina *GenericErrorPage.aspx.*

![Pagina di errore](web-config-transformations/_static/image2.png)

Per visualizzare il log degli errori, sostituire tutti gli elementi nell'URL dopo `http://localhost:51130/elmah.axd`il numero di porta con *elmah.axd* (ad esempio, ) e premere INVIO:

![Pagina ELMAH](web-config-transformations/_static/image3.png)

Non dimenticare di impostare di nuovo l'elemento `customErrors` sulla modalità "RemoteOnly" quando hai finito.

Nel computer di sviluppo è conveniente consentire l'accesso gratuito alla pagina del log degli errori, ma nell'ambiente di produzione che sarebbe un rischio per la sicurezza. Per il sito di produzione, si desidera aggiungere una regola di autorizzazione che limita l'accesso al log degli errori agli amministratori e per assicurarsi che la restrizione che si desidera eseguire anche il test e la gestione temporanea. Si tratta pertanto di un'altra modifica che si desidera implementare ogni volta che si distribuisce una build di rilascio e pertanto appartiene al file *Web.Release.config.*

Aprire *Web.Release.config* e `location` aggiungere un nuovo `configuration` elemento immediatamente prima del tag di chiusura, come illustrato di seguito.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Il `Transform` valore dell'attributo `location` "Insert" fa sì che questo `location` elemento venga aggiunto come elemento di pari livello a tutti gli elementi esistenti nel file *Web.config.* Esiste già un `location` elemento che specifica le regole di autorizzazione per la pagina **Aggiorna crediti.**

Ora è possibile visualizzare in anteprima la trasformazione per assicurarsi di averla codificata correttamente.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *Web.Release.config* e **scegliere Anteprima trasformazione**.

![Menu Anteprima trasformazione](web-config-transformations/_static/image4.png)

Viene visualizzata una pagina che mostra il file *Web.config* di sviluppo a sinistra e l'aspetto del file *Web.config* distribuito a destra, con le modifiche evidenziate.

![Anteprima della trasformazione di debug](web-config-transformations/_static/image5.png)

![Anteprima della trasformazione della posizione](web-config-transformations/_static/image6.png)

Nell'anteprima, è possibile notare alcune modifiche aggiuntive per le quali non sono state scritte trasformazioni: in genere comportano la rimozione di spazi vuoti che non influiscono sulla funzionalità.

Quando si testa il sito dopo la distribuzione, si verificherà anche che la regola di autorizzazione sia efficace.

> [!NOTE] 
> 
> **Nota sulla sicurezza** Non visualizzare mai i dettagli dell'errore al pubblico in un'applicazione di produzione o archiviare tali informazioni in un luogo pubblico. Gli utenti malintenzionati possono utilizzare le informazioni sugli errori per individuare le vulnerabilità in un sito. Se si utilizza ELMAH nella propria applicazione, configurare ELMAH per ridurre al minimo i rischi per la sicurezza. L'esempio ELMAH in questa esercitazione non deve essere considerato una configurazione consigliata. Si tratta di un esempio che è stato scelto per illustrare come gestire una cartella in cui l'applicazione deve essere in grado di creare file. Per ulteriori informazioni, vedere [protezione dell'endpoint ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un'impostazione che verrà gestita nei file di trasformazione del profilo di pubblicazione

Uno scenario comune consiste nel disporre di impostazioni del file *Web.config* che devono essere diverse in ogni ambiente in cui si esegue la distribuzione. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe richiedere un endpoint diverso negli ambienti di test e produzione. L'applicazione Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile nelle pagine di un sito che indica in quale ambiente ci si trova, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione aggiungerà "(Dev)" o "(Test)" all'intestazione principale nella pagina master *Site.Master:*

![Indicatore di ambiente](web-config-transformations/_static/image7.png)

L'indicatore di ambiente viene omesso quando l'applicazione è in esecuzione in gestione temporanea o in produzione.

Le pagine Web della Contoso University `appSettings` leggono un valore impostato nel file *Web.config* per determinare in quale ambiente viene eseguita l'applicazione:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Il valore deve essere "Test" nell'ambiente di test e "Prod" per la gestione temporanea e la produzione.

Il codice seguente in un file di trasformazione implementerà questa trasformazione:The following code in a transform file will implement this transformation:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Il `xdt:Transform` valore dell'attributo "SetAttributes" indica che lo scopo di questa trasformazione consiste nel modificare i valori degli attributi di un elemento esistente nel file *Web.config.* Il `xdt:Locator` valore dell'attributo "Match(key)" indica che l'elemento da modificare è quello il cui `key` attributo corrisponde all'attributo `key` specificato qui. L'unico altro `add` attributo `value`dell'elemento è , ovvero ciò che verrà modificato nel file *Web.config* distribuito. Il codice riportato `value` di `Environment` `appSettings` seguito fa sì che l'attributo dell'elemento venga impostato su "Test" nel file *Web.config* distribuito.

Questa trasformazione appartiene ai file di trasformazione del profilo di pubblicazione, che non sono ancora stati creati. I file di trasformazione che implementano questa modifica verranno creati quando si creano i profili di pubblicazione per gli ambienti di test, gestione temporanea e produzione. Questa operazione verrà eseguito nella [distribuzione in IIS](deploying-to-iis.md) e nelle esercitazioni sull'ambiente di [produzione.](deploying-to-production.md)

> [!NOTE]
> Poiché questa impostazione si trova nell'elemento, `<appSettings>` è possibile specificare la trasformazione quando si esegue la distribuzione alle app Web nel servizio app di Azure Vedere Specifica delle impostazioni [Web.config in Azure](#watransforms) più indietro in questo argomento.

## <a name="setting-connection-strings"></a>Impostazione delle stringhe di connessione

Anche se il file di trasformazione predefinito contiene un esempio che illustra come aggiornare una stringa di connessione, nella maggior parte dei casi non è necessario impostare trasformazioni della stringa di connessione, perché è possibile specificare le stringhe di connessione nel profilo di pubblicazione. Questa operazione verrà eseguito nella [distribuzione in IIS](deploying-to-iis.md) e nelle esercitazioni sull'ambiente di [produzione.](deploying-to-production.md)

## <a name="summary"></a>Riepilogo

A questo punto è stato fatto quanto più possibile con le trasformazioni *Web.config* prima di creare i profili di pubblicazione e si è visualizzata un'anteprima di ciò che sarà nel file Web.config distribuito.

![Anteprima della trasformazione della posizione](web-config-transformations/_static/image8.png)

Nell'esercitazione seguente, è necessario eseguire l'attenzione delle attività di configurazione della distribuzione che richiedono l'impostazione delle proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere Utilizzo delle [trasformazioni Web.config per modificare le impostazioni nel file Web.config o nel file app.config](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) di destinazione durante la distribuzione nella mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET.

> [!div class="step-by-step"]
> [Successivo](preparing-databases.md)
> [precedente](project-properties.md)
