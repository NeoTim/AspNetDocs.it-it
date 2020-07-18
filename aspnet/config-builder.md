---
uid: config-builder
title: Generatori di configurazioni per ASP.NET
author: rick-anderson
description: Informazioni su come ottenere dati di configurazione da origini diverse da web.config valori da origini esterne.
ms.author: riande
ms.date: 7/17/2020
msc.type: content
ms.openlocfilehash: 1f95efcceb2ecf33fece12174cecf65cd8b27675
ms.sourcegitcommit: 000cbcd1de66fe8a766f203ef2d6f009e4435f53
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/16/2020
ms.locfileid: "86424799"
---
# <a name="configuration-builders-for-aspnet"></a>Generatori di configurazioni per ASP.NET

Di [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)

I generatori di configurazioni forniscono un meccanismo moderno e agile per le app ASP.NET per ottenere i valori di configurazione da origini esterne.

Generatori di configurazione:

* Sono disponibili in .NET Framework 4.7.1 e versioni successive.
* Fornire un meccanismo flessibile per la lettura dei valori di configurazione.
* Risolvere alcune delle esigenze di base delle app quando si spostano in un ambiente contenitore e basato sul cloud.
* Può essere usato per migliorare la protezione dei dati di configurazione mediante il disegno da origini precedentemente non disponibili, ad esempio Azure Key Vault e variabili di ambiente, nel sistema di configurazione .NET.

## <a name="keyvalue-configuration-builders"></a>Costruttori di configurazione chiave/valore

Uno scenario comune che può essere gestito dai generatori di configurazione è fornire un meccanismo di sostituzione chiave/valore di base per le sezioni di configurazione che seguono un modello chiave/valore. Il concetto .NET Framework di ConfigurationBuilders non è limitato a sezioni o modelli di configurazione specifici. Tuttavia, molti dei generatori di configurazioni in `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funzionano nel modello chiave/valore.

## <a name="keyvalue-configuration-builders-settings"></a>Impostazioni del Builder di configurazione chiave/valore

Le impostazioni seguenti si applicano a tutti i generatori di configurazione chiave/valore in `Microsoft.Configuration.ConfigurationBuilders` .

### <a name="mode"></a>Mode

I generatori di configurazione usano un'origine esterna di informazioni chiave/valore per popolare gli elementi chiave/valore selezionati del sistema di configurazione. In particolare, `<appSettings/>` le `<connectionStrings/>` sezioni e ricevono un trattamento speciale dai generatori di configurazione. I generatori funzionano in tre modalità:

* `Strict`: La modalità predefinita. In questa modalità, il generatore di configurazione opera solo in sezioni di configurazione basate su chiave/valore ben note. `Strict`la modalità enumera ogni chiave nella sezione. Se viene trovata una chiave corrispondente nell'origine esterna:

   * I generatori di configurazioni sostituiscono il valore nella sezione di configurazione risultante con il valore dell'origine esterna.
* `Greedy`: Questa modalità è strettamente correlata alla `Strict` modalità. Anziché essere limitati alle chiavi già presenti nella configurazione originale:

  * I generatori di configurazioni aggiungono tutte le coppie chiave/valore dall'origine esterna alla sezione di configurazione risultante.

* `Expand`: Opera sul codice XML non elaborato prima che venga analizzato in un oggetto sezione di configurazione. Può essere considerato come un'espansione di token in una stringa. Qualsiasi parte della stringa XML non elaborata corrispondente al modello `${token}` è un candidato per l'espansione del token. Se non viene trovato alcun valore corrispondente nell'origine esterna, il token non viene modificato. I generatori in questa modalità non sono limitati alle `<appSettings/>` `<connectionStrings/>` sezioni e.

Il markup seguente da *web.config* Abilita [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` modalità:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Il codice seguente legge `<appSettings/>` e `<connectionStrings/>` illustrato nel file di *web.config* precedente:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Il codice precedente imposterà i valori della proprietà su:

* Valori nel file di *web.config* se le chiavi non sono impostate nelle variabili di ambiente.
* Valori della variabile di ambiente, se impostati.

Ad esempio, conterrà `ServiceID` :

* "Valore ServiceID da web.config", se la variabile di ambiente `ServiceID` non è impostata.
* Valore della variabile di `ServiceID` ambiente, se impostato.

Nell'immagine seguente vengono illustrate le `<appSettings/>` chiavi e i valori del set di file di *web.config* precedente nell'editor dell'ambiente:

![Editor dell'ambiente](config-builder/static/env.png)

Nota: potrebbe essere necessario chiudere e riavviare Visual Studio per visualizzare le modifiche apportate alle variabili di ambiente.

### <a name="prefix-handling"></a>Gestione dei prefissi

I prefissi chiave possono semplificare l'impostazione delle chiavi perché:

* La configurazione del .NET Framework è complessa e nidificata.
* Le origini chiave/valore esterne sono comunemente Basic e flat per natura. Ad esempio, le variabili di ambiente non sono annidate.

Usare uno degli approcci seguenti per inserire `<appSettings/>` e `<connectionStrings/>` nella configurazione tramite le variabili di ambiente:

* Con la `EnvironmentConfigBuilder` `Strict` modalità predefinita e i nomi di chiave appropriati nel file di configurazione. Il codice e il markup precedenti prendono questo approccio. Con questo approccio **non** è possibile avere chiavi denominate in modo identico sia in `<appSettings/>` che in `<connectionStrings/>` .
* Usare due `EnvironmentConfigBuilder` in `Greedy` modalità con i prefissi distinti e `stripPrefix` . Con questo approccio, l'app può leggere `<appSettings/>` e `<connectionStrings/>` senza dover aggiornare il file di configurazione. Nella sezione successiva, [stripPrefix](#stripprefix), viene illustrato come eseguire questa operazione.
* Usare due `EnvironmentConfigBuilder` in `Greedy` modalità con i prefissi distinti. Con questo approccio non è possibile avere nomi di chiave duplicati perché i nomi delle chiavi devono essere diversi per prefisso.  Ad esempio:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Con il markup precedente, è possibile usare la stessa origine di chiave/valore flat per popolare la configurazione per due sezioni diverse.

La figura seguente mostra i `<appSettings/>` valori e le `<connectionStrings/>` chiavi del set di file di *web.config* precedente nell'editor dell'ambiente:

![Editor dell'ambiente](config-builder/static/prefix.png)

Il codice seguente legge le `<appSettings/>` `<connectionStrings/>` chiavi e/valori contenuti nel file di *web.config* precedente:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Il codice precedente imposterà i valori della proprietà su:

* Valori nel file di *web.config* se le chiavi non sono impostate nelle variabili di ambiente.
* Valori della variabile di ambiente, se impostati.

Ad esempio, usando il file di *web.config* precedente, le chiavi e i valori nell'immagine precedente dell'editor dell'ambiente e il codice precedente, vengono impostati i valori seguenti:

|  Chiave              | Valore |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID dalle variabili env|
|    AppSetting_default            | AppSetting_default valore da ENV |
|       ConnStr_default         | ConnStr_default Val da ENV|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: booleano, il valore predefinito è `false` . 

Il markup XML precedente separa le impostazioni dell'app dalle stringhe di connessione, ma richiede che tutte le chiavi nel file di *web.config* usino il prefisso specificato. Il prefisso, ad esempio, `AppSetting` deve essere aggiunto alla `ServiceID` chiave ("AppSetting_ServiceID"). Con `stripPrefix` , il prefisso non viene utilizzato nel file di *web.config* . Il prefisso è obbligatorio nell'origine del generatore di configurazione (ad esempio, nell'ambiente). Si prevede che la maggior parte degli sviluppatori utilizzerà `stripPrefix` .

Le applicazioni in genere eliminano il prefisso. Il *web.config* seguente rimuove il prefisso:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Nel file di *web.config* precedente, la `default` chiave si trova sia in `<appSettings/>` che in `<connectionStrings/>` .

La figura seguente mostra i `<appSettings/>` valori e le `<connectionStrings/>` chiavi del set di file di *web.config* precedente nell'editor dell'ambiente:

![Editor dell'ambiente](config-builder/static/prefix.png)

Il codice seguente legge le `<appSettings/>` `<connectionStrings/>` chiavi e/valori contenuti nel file di *web.config* precedente:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Il codice precedente imposterà i valori della proprietà su:

* Valori nel file di *web.config* se le chiavi non sono impostate nelle variabili di ambiente.
* Valori della variabile di ambiente, se impostati.

Ad esempio, usando il file di *web.config* precedente, le chiavi e i valori nell'immagine precedente dell'editor dell'ambiente e il codice precedente, vengono impostati i valori seguenti:

|  Chiave              | Valore |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID dalle variabili env|
|    default            | AppSetting_default valore da ENV |
|    default         | ConnStr_default Val da ENV|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Stringa, il valore predefinito è`@"\$\{(\w+)\}"`

Il `Expand` comportamento dei generatori Cerca nel codice XML non elaborato i token che hanno un aspetto simile a `${token}` . La ricerca viene eseguita con l'espressione regolare predefinita `@"\$\{(\w+)\}"` . Il set di caratteri che corrisponde `\w` è più rigoroso di XML e molte origini di configurazione consentono. Usare `tokenPattern` quando sono necessari più caratteri del `@"\$\{(\w+)\}"` nome del token.

`tokenPattern`Stringa

* Consente agli sviluppatori di modificare l'espressione regolare utilizzata per la corrispondenza dei token.
* Non viene eseguita alcuna convalida per assicurarsi che si tratta di un'espressione regolare ben formata e non pericolosa.
* Deve contenere un gruppo Capture. L'intera espressione regolare deve corrispondere all'intero token. La prima acquisizione deve essere il nome del token da cercare nell'origine della configurazione.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Generatori di configurazioni in Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* È il più semplice dei generatori di configurazioni.
* Legge i valori dall'ambiente.
* Non dispone di opzioni di configurazione aggiuntive.
* Il `name` valore dell'attributo è arbitrario.

**Nota:** In un ambiente contenitore Windows le variabili impostate in fase di esecuzione vengono inserite solo nell'ambiente di processo EntryPoint. Le app eseguite come servizio o un processo non EntryPoint non prelevano queste variabili a meno che non vengano inserite in altro modo tramite un meccanismo nel contenitore. Per [IIS](https://github.com/Microsoft/iis-docker/pull/41) / i contenitori basati su[ASP.NET](https://github.com/Microsoft/aspnet-docker)IIS, la versione corrente di [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) gestisce solo questa operazione in *DefaultAppPool* . Altre varianti di contenitori basate su Windows possono dover sviluppare un proprio meccanismo di inserimento per i processi non EntryPoint.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Non archiviare mai password, stringhe di connessione sensibili o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o il test.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Questo generatore di configurazioni fornisce una funzionalità simile a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) può essere usato nei progetti .NET Framework, ma è necessario specificare un file Secrets. In alternativa, è possibile definire la `UserSecretsId` proprietà nel file di progetto e creare il file Secrets RAW nel percorso corretto per la lettura. Per escludere le dipendenze esterne dal progetto, il file del segreto è formattato in formato XML. La formattazione XML è un dettaglio di implementazione e il formato non deve essere considerato attendibile. Se è necessario condividere un *secrets.jssu* file con i progetti .NET Core, provare a usare [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). Il `SimpleJsonConfigBuilder` formato di .NET Core deve anche essere considerato un dettaglio di implementazione soggetto a modifiche.

Attributi di configurazione per `UserSecretsConfigBuilder` :

* `userSecretsId`-Questo è il metodo preferito per l'identificazione di un file di segreti XML. Funziona in modo simile a .NET Core, che usa una `UserSecretsId` proprietà del progetto per archiviare questo identificatore. La stringa deve essere univoca e non deve necessariamente essere un GUID. Con questo attributo, `UserSecretsConfigBuilder` Cerca in un percorso locale noto ( `%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml` ) per un file Secrets che appartiene a questo identificatore.
* `userSecretsFile`: Attributo facoltativo che specifica il file che contiene i segreti. Il `~` carattere può essere usato all'inizio per fare riferimento alla radice dell'applicazione. È necessario specificare questo attributo o l' `userSecretsId` attributo. Se vengono specificati entrambi, `userSecretsFile` avrà la precedenza.
* `optional`: booleano, valore predefinito `true` -impedisce un'eccezione se non è possibile trovare il file dei segreti. 
* Il `name` valore dell'attributo è arbitrario.

Il file Secrets ha il formato seguente:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) legge i valori archiviati nel [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName`è obbligatorio (il nome dell'insieme di credenziali o un URI per l'insieme di credenziali). Gli altri attributi consentono di controllare l'insieme di credenziali a cui connettersi, ma sono necessari solo se l'applicazione non è in esecuzione in un ambiente che funziona con `Microsoft.Azure.Services.AppAuthentication` . La libreria di autenticazione dei servizi di Azure viene usata per selezionare automaticamente le informazioni di connessione dall'ambiente di esecuzione, se possibile. È possibile eseguire l'override della selezione automatica delle informazioni di connessione fornendo una stringa di connessione.

* `vaultName`: Obbligatorio se `uri` non è stato specificato. Specifica il nome dell'insieme di credenziali nella sottoscrizione di Azure da cui leggere le coppie chiave/valore.
* `connectionString`-Una stringa di connessione utilizzabile da [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri`-Si connette ad altri provider di Key Vault con il `uri` valore specificato. Se non specificato, Azure ( `vaultName` ) è il provider dell'insieme di credenziali.
* `version`-Azure Key Vault fornisce una funzionalità di controllo delle versioni per i segreti. Se `version` si specifica, il generatore recupera solo i segreti che corrispondono a questa versione.
* `preloadSecretNames`Per impostazione predefinita, questo generatore esegue una query su tutti i nomi di chiave nell' **insieme** di credenziali delle chiavi quando viene inizializzato. Per evitare la lettura di tutti i valori di chiave, impostare questo attributo su `false` . Impostando questa opzione su vengono `false` letti i segreti uno alla volta. La lettura di segreti uno alla volta può essere utile se l'insieme di credenziali consente l'accesso "Get", ma non l'accesso "list". **Nota:** Quando `Greedy` si usa la modalità, `preloadSecretNames` deve essere `true` (impostazione predefinita).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) è un generatore di configurazioni di base che usa i file di una directory come origine dei valori. Il nome di un file è la chiave e il contenuto è il valore. Questo generatore di configurazione può essere utile quando viene eseguito in un ambiente contenitore orchestrato. Sistemi come Docker Swarm e Kubernetes forniscono `secrets` ai contenitori Windows orchestrati in questa modalità chiave per file.

Dettagli attributo:

* `directoryPath` - Obbligatorio. Specifica un percorso in cui cercare i valori. Per impostazione predefinita, i segreti Docker per Windows vengono archiviati nella directory *C:\ProgramData\Docker\secrets*
* `ignorePrefix`-I file che iniziano con questo prefisso vengono esclusi. Il valore predefinito è "ignore".
* `keyDelimiter`-Il valore predefinito è `null` . Se specificato, il generatore di configurazioni attraversa più livelli della directory, costituendo i nomi delle chiavi con questo delimitatore. Se questo valore è `null` , il generatore di configurazione esamina solo il livello principale della directory.
* `optional`-Il valore predefinito è `false` . Specifica se il generatore di configurazione deve causare errori se la directory di origine non esiste.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Non archiviare mai password, stringhe di connessione sensibili o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o il test.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

I progetti .NET Core usano spesso file JSON per la configurazione. Il generatore [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) consente di usare i file JSON di .NET Core nel .NET Framework. Questo generatore di configurazioni fornisce un mapping di base da un'origine chiave/valore flat in specifiche aree chiave/valore della configurazione .NET Framework. Questo generatore di configurazione **non** fornisce le configurazioni gerarchiche. Il file di backup JSON è simile a un dizionario, non a un oggetto gerarchico complesso. È possibile utilizzare un file gerarchico a più livelli. Questo provider `flatten` è la profondità aggiungendo il nome della proprietà a ogni livello utilizzando `:` come delimitatore.

Dettagli attributo:

* `jsonFile` - Obbligatorio. Specifica il file JSON da cui leggere. Il `~` carattere può essere usato all'inizio per fare riferimento alla radice dell'app.
* `optional`-Booleano, il valore predefinito è `true` . Impedisce la generazione di eccezioni se il file JSON non è stato trovato.
* `jsonMode` - `[Flat|Sectional]`. `Flat` è l'impostazione predefinita. Quando `jsonMode` è `Flat` , il file JSON è un'unica origine di chiave/valore flat. `EnvironmentConfigBuilder`E `AzureKeyVaultConfigBuilder` sono inoltre singole origini chiave/valore flat. Quando `SimpleJsonConfigBuilder` è configurato in `Sectional` modalità:

  * Il file JSON viene diviso concettualmente solo al livello principale in più dizionari.
  * Ognuno dei dizionari viene applicato solo alla sezione di configurazione che corrisponde al nome della proprietà di primo livello associato. Ad esempio:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="configuration-builders-order"></a>Ordine generatori di configurazioni

Vedere l' [ordine di esecuzione ConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) nel repository GitHub [ASPNET/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) .

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementazione di un generatore di configurazione chiave/valore personalizzato

Se i generatori di configurazione non soddisfano le proprie esigenze, è possibile scriverne uno personalizzato. La `KeyValueConfigBuilder` classe base gestisce le modalità di sostituzione e la maggior parte dei problemi relativi al prefisso. Un progetto di implementazione deve solo:

* Ereditare dalla classe di base e implementare un'origine di base di coppie chiave/valore tramite `GetValue` e `GetAllValues` :
* Aggiungere il [Microsoft.Configuration.ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al progetto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

La `KeyValueConfigBuilder` classe base fornisce gran parte del lavoro e del comportamento coerente nei generatori di configurazione chiave/valore.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Repository GitHub di Configuration Builders](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticazione da servizio a servizio a Azure Key Vault con .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
