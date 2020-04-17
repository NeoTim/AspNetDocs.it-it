---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profili, temi e web part Documenti Microsoft
author: rick-anderson
description: La configurazione e la strumentazione sono state apportate in ASP.NET 2.0. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione pr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 4bc98cca226a0bd9bd766a21e88b0facf2a4b610
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543639"
---
# <a name="profiles-themes-and-web-parts"></a>Profili, temi e Web part

da parte [di Microsoft](https://github.com/microsoft)

> La configurazione e la strumentazione sono state apportate in ASP.NET 2.0. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione a livello di codice. Inoltre, esistono molte nuove impostazioni di configurazione che consentono nuove configurazioni e strumentazione.

ASP.NET 2.0 rappresenta un miglioramento sostanziale nell'area dei siti Web personalizzati. Oltre alle funzionalità di appartenenza già coperte, ASP.NET profili, temi e Web part migliorano in modo significativo la personalizzazione nei siti Web.

## <a name="aspnet-profiles"></a>Profili ASP.NET

ASP.NET profili sono simili alle sessioni. La differenza è che un profilo è persistente mentre una sessione viene persa quando il browser viene chiuso. Un'altra grande differenza tra sessioni e profili è che i profili sono fortemente tipizzati, fornendo quindi IntelliSense durante il processo di sviluppo.

Un profilo viene definito nel file di configurazione del computer o nel file web.config per l'applicazione. Non è possibile definire un profilo in un file web.config delle sottocartelle. Il codice seguente definisce un profilo per archiviare il nome e il cognome dei visitatori del sito Web.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Il tipo di dati predefinito per una proprietà del profilo è System.String.The default data type for a profile property is System.String. Nell'esempio precedente non è stato specificato alcun tipo di dati. Pertanto le proprietà FirstName e LastName sono entrambe di tipo String. Come accennato in precedenza, le proprietà del profilo sono fortemente tipizzate. Il codice seguente aggiunge una nuova proprietà per age che è di tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

I profili vengono in genere utilizzati con l'autenticazione basata su form ASP.NET. Se utilizzato in combinazione con l'autenticazione basata su form, ogni utente dispone di un profilo separato associato al proprio ID utente. Tuttavia, è anche possibile consentire l'utilizzo di &lt;profili&gt; in un'applicazione anonima utilizzando l'elemento anonymousIdentification nel file di configurazione insieme all'attributo **allowAnonymous,** come illustrato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Quando un utente anonimo esplora il sito, ASP.NET crea un'istanza di **ProfileCommon** per l'utente. Questo profilo utilizza un ID univoco memorizzato in un cookie nel browser per identificare l'utente come visitatore univoco. In questo modo, è possibile memorizzare le informazioni del profilo per gli utenti che navigano in modo anonimo.

## <a name="profile-groups"></a>Gruppi di profili

È possibile raggruppare le proprietà dei profili. Raggruppando le proprietà, è possibile simulare più profili per un'applicazione specifica.

La configurazione seguente configura una proprietà FirstName e LastName per due gruppi. Acquirenti e Prospettive.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

È quindi possibile impostare le proprietà di un particolare gruppo come segue:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Memorizzazione di oggetti complessi

Finora, gli esempi che abbiamo trattato hanno archiviato tipi di dati semplici in un profilo. È anche possibile archiviare tipi di dati complessi in un profilo specificando il metodo di serializzazione utilizzando l'attributo **serializeAs** nel modo seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In questo caso, il tipo è PurchaseInvoice. La classe PurchaseInvoice deve essere contrassegnata come serializzabile e può contenere un numero qualsiasi di proprietà. Ad esempio, se PurchaseInvoice dispone di una proprietà denominata **NumItemsPurchased**, è possibile fare riferimento a tale proprietà nel codice come segue:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Ereditarietà del profilo

È possibile creare un profilo da utilizzare in più applicazioni. Creando una classe di profilo che deriva da ProfileBase, è possibile riutilizzare un profilo in diverse applicazioni utilizzando l'attributo inherits come illustrato di seguito:By creating a profile class that derives from ProfileBase, you can reuse a profile in several applications by using the **inherits** attribute as shown below:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In questo caso, la classe PurchasingProfile sarebbe simile alla seguente:In this case, the class **PurchasingProfile** would look like so:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Provider di profili

ASP.NET profili utilizzano il modello di provider. Il provider predefinito archivia le informazioni in\_un database di SQL Server Express nella cartella App Data dell'applicazione Web utilizzando il provider SqlProfileProvider. Se il database non esiste, ASP.NET lo creerà automaticamente quando il profilo tenta di archiviare le informazioni.

In alcuni casi, tuttavia, è possibile sviluppare il proprio provider di profili. La funzionalità ASP.NET profilo consente di utilizzare facilmente provider diversi.

Si crea un provider di profili personalizzato quando:You create a custom profile provider when:

- È necessario archiviare le informazioni sui profili in un'origine dati, ad esempio in un database FoxPro o in un database Oracle, che non è supportato dai provider di profili inclusi in .NET Framework.
- È necessario gestire le informazioni sui profili utilizzando uno schema di database diverso dallo schema di database utilizzato dai provider inclusi in .NET Framework. Un esempio comune è che si desidera integrare le informazioni del profilo con i dati utente in un database di SQL Server esistente.

### <a name="required-classes"></a>Classi obbligatorie

Per implementare un provider di profili, creare una classe che eredita la classe astratta System.Web.Profile.ProfileProvider . La classe astratta **ProfileProvider** eredita a sua volta la classe astratta System.Configuration.SettingsProvider , che eredita la classe astratta System.Configuration.Provider.ProviderBase . A causa di questa catena di ereditarietà, oltre ai membri obbligatori della classe **ProfileProvider,** è necessario implementare i membri obbligatori delle classi **SettingsProvider** e **ProviderBase.**

Nelle tabelle riportate di seguito vengono descritte le proprietà e i metodi che è necessario implementare dalle classi astratte **ProviderBase**, **SettingsProvider**e **ProfileProvider** .

### <a name="providerbase-members"></a>Membri ProviderBase

| **Membro** | **Descrizione** |
| --- | --- |
| Initialize (metodo) | Accetta come input il nome dell'istanza del provider e un NameValueCollection delle impostazioni di configurazione. Utilizzato per impostare le opzioni e i valori delle proprietà per l'istanza del provider, inclusi i valori specifici dell'implementazione e le opzioni specificate nella configurazione del computer o nel file Web.config. |

### <a name="settingsprovider-members"></a>Membri SettingsProvider

| **Membro** | **Descrizione** |
| --- | --- |
| ApplicationName (proprietà) | Il nome dell'applicazione archiviato con ogni profilo. Il provider di profili utilizza il nome dell'applicazione per archiviare le informazioni sul profilo separatamente per ogni applicazione. Ciò consente a più applicazioni ASP.NET di utilizzare la stessa origine dati senza conflitti se lo stesso nome utente viene creato in applicazioni diverse. In alternativa, più applicazioni ASP.NET possono condividere un'origine dati del profilo specificando lo stesso nome dell'applicazione. |
| GetPropertyValues (metodo) | Accetta come input un SettingsContext e un SettingsPropertyCollection oggetto. **Il SettingsContext** fornisce informazioni sull'utente. È possibile utilizzare le informazioni come chiave primaria per recuperare le informazioni sulle proprietà del profilo per l'utente. Utilizzare l'oggetto **SettingsContext** per ottenere il nome utente e se l'utente è autenticato o anonimo. Il **SettingsPropertyCollection** contiene una raccolta di SettingsProperty oggetti. Ogni **SettingsProperty** oggetto fornisce il nome e il tipo della proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e se la proprietà è di sola lettura. Il **GetPropertyValues** metodo popola un SettingsPropertyValueCollection con SettingsPropertyValue oggetti basati sul **SettingsProperty** oggetti forniti come input. I valori dell'origine dati per l'utente specificato vengono assegnati alle proprietà PropertyValue per ogni oggetto **SettingsPropertyValue** e viene restituita l'intera raccolta. La chiamata al metodo aggiorna anche il valore LastActivityDate per il profilo utente specificato alla data e all'ora correnti. |
| SetPropertyValues (metodo) | Accetta come input un **SettingsContext** e un **SettingsPropertyValueCollection** oggetto. **Il SettingsContext** fornisce informazioni sull'utente. È possibile utilizzare le informazioni come chiave primaria per recuperare le informazioni sulle proprietà del profilo per l'utente. Utilizzare l'oggetto **SettingsContext** per ottenere il nome utente e se l'utente è autenticato o anonimo. Il **SettingsPropertyValueCollection** contiene una raccolta di **SettingsPropertyValue** oggetti. Ogni **SettingsPropertyValue** oggetto fornisce il nome, tipo e valore della proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e se la proprietà è di sola lettura. Il **SetPropertyValues** metodo aggiorna i valori delle proprietà del profilo nell'origine dati per l'utente specificato. La chiamata al metodo aggiorna anche i valori **LastActivityDate** e LastUpdatedDate per il profilo utente specificato alla data e all'ora correnti. |

### <a name="profileprovider-members"></a>Membri ProfileProvider

| **Membro** | **Descrizione** |
| --- | --- |
| DeleteProfiles (metodo) | Accetta come input una matrice di stringhe di nomi utente ed elimina dall'origine dati tutte le informazioni sul profilo e i valori delle proprietà per i nomi specificati in cui il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName.** Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni di eliminazione in una transazione e eseguire il rollback della transazione e generare un'eccezione se un'operazione di eliminazione ha esito negativo. |
| DeleteProfiles (metodo) | Accetta come input una raccolta di ProfileInfo oggetti ed elimina dall'origine dati tutte le informazioni sul profilo e i valori delle proprietà per ogni profilo in cui il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni di eliminazione in una transazione ed eseguire il rollback della transazione e generare un'eccezione se un'operazione di eliminazione ha esito negativo. |
| DeleteInactiveProfiles (metodo) | Accetta come input un valore ProfileAuthenticationOption e un oggetto DateTime ed elimina dall'origine dati tutte le informazioni sul profilo e i valori delle proprietà in cui la data dell'ultima attività è minore o uguale alla data e all'ora specificate e in cui il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName.** Il parametro **ProfileAuthenticationOption** consente di specificare se devono essere eliminati solo i profili anonimi, solo i profili autenticati o tutti i profili. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni di eliminazione in una transazione ed eseguire il rollback della transazione e generare un'eccezione se un'operazione di eliminazione ha esito negativo. |
| GetAllProfiles (metodo) | Accetta come input un valore **ProfileAuthenticationOption,** un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un numero intero che verrà impostato sul conteggio totale dei profili. Restituisce un ProfileInfoCollection che contiene **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Il parametro **ProfileAuthenticationOption** consente di specificare se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. I risultati restituiti dal **metodo GetAllProfiles** sono vincolati dai valori dell'indice e delle dimensioni della pagina. Il valore della dimensione della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica quale pagina di risultati restituire, dove 1 identifica la prima pagina. Il parametro per i record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) impostato sul numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce . |
| GetAllInactiveProfiles (metodo) | Accetta come input un valore **ProfileAuthenticationOption,** un oggetto **DateTime,** un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un numero intero che verrà impostato sul conteggio totale dei profili. Restituisce un **ProfileInfoCollection** che contiene **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale al **DateTime** specificato e in cui il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Il parametro **ProfileAuthenticationOption** consente di specificare se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. I risultati restituiti dal **metodo GetAllInactiveProfiles** sono vincolati dai valori dell'indice e delle dimensioni della pagina. Il valore della dimensione della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica quale pagina di risultati restituire, dove 1 identifica la prima pagina. Il parametro per i record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) impostato sul numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce . |
| FindProfilesByUserName (metodo) | Accetta come input un valore **ProfileAuthenticationOption,** una stringa contenente un nome utente, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un numero intero che verrà impostato sul conteggio totale dei profili. Restituisce un **ProfileInfoCollection** che contiene **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui il nome utente corrisponde al nome utente specificato e in cui il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Il parametro **ProfileAuthenticationOption** consente di specificare se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. Se l'origine dati supporta funzionalità di ricerca aggiuntive, ad esempio i caratteri jolly, è possibile fornire funzionalità di ricerca più estese per i nomi utente. I risultati restituiti dal **metodo FindProfilesByUserName** sono vincolati dai valori dell'indice e delle dimensioni della pagina. Il valore della dimensione della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica quale pagina di risultati restituire, dove 1 identifica la prima pagina. Il parametro per i record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) impostato sul numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce . |
| FindInactiveProfilesByUserName (metodo) | Accetta come input un valore **ProfileAuthenticationOption,** una stringa contenente un nome utente, un oggetto **DateTime,** un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un numero intero che verrà impostato sul conteggio totale dei profili. Restituisce un **ProfileInfoCollection** che contiene **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui il nome utente corrisponde al nome utente specificato, dove la data dell'ultima attività è minore o uguale al **DateTime**specificato e in cui il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Il parametro **ProfileAuthenticationOption** consente di specificare se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. Se l'origine dati supporta funzionalità di ricerca aggiuntive, ad esempio i caratteri jolly, è possibile fornire funzionalità di ricerca più estese per i nomi utente. I risultati restituiti dal metodo **FindInactiveProfilesByUserName** sono vincolati dai valori dell'indice e delle dimensioni della pagina. Il valore della dimensione della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica quale pagina di risultati restituire, dove 1 identifica la prima pagina. Il parametro per i record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) impostato sul numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce . |
| GetNumberOfInActiveProfiles (metodo) | Accetta come input un valore **ProfileAuthenticationOption** e un oggetto **DateTime** e restituisce un conteggio di tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale al **valore DateTime** specificato e in cui il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName.** Il parametro **ProfileAuthenticationOption** consente di specificare se devono essere conteggiati solo i profili anonimi, solo i profili autenticati o tutti i profili. |

### <a name="applicationname"></a>ApplicationName

Poiché i provider di profili archiviano le informazioni sui profili separatamente per ogni applicazione, è necessario assicurarsi che lo schema di dati includa il nome dell'applicazione e che anche le query e gli aggiornamenti includano il nome dell'applicazione. Ad esempio, il comando seguente viene utilizzato per recuperare un valore di proprietà da un database in base al nome utente e se il profilo è anonimo e assicura che il **ApplicationName** valore è incluso nella query.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET Temi

## <a name="what-are-aspnet-20-themes"></a>Cosa sono ASP.NET 2.0 temi?

Uno degli aspetti più importanti di un'applicazione Web è un aspetto coerente in tutto il sito. ASP.NET 1.x gli sviluppatori in genere utilizzano i fogli di stile CSS (Cascading Style Sheets) per implementare un aspetto coerente. ASP.NET 2.0 temi migliorano significativamente su CSS perché danno allo sviluppatore ASP.NET la possibilità di definire l'aspetto dei controlli server ASP.NET così come gli elementi HTML. ASP.NET temi possono essere applicati a singoli controlli, a una pagina Web specifica o a un'intera applicazione Web. I temi utilizzano una combinazione di file CSS, un file skin facoltativo e una directory Images facoltativa se sono necessarie immagini. Il file skin controlla l'aspetto visivo dei controlli server ASP.NET.

## <a name="where-are-themes-stored"></a>Dove sono memorizzati i temi?

La posizione in cui vengono archiviati i temi varia in base all'ambito. I temi che possono essere applicati a qualsiasi applicazione vengono archiviati nella cartella seguente:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema specifico di una determinata applicazione `App\_Themes\<Theme\_Name>` viene memorizzato in una directory nella radice del sito Web.

> [!NOTE]
> Un file di interfaccia deve modificare solo le proprietà del controllo server che influiscono sull'aspetto.

Un tema globale è un tema che può essere applicato a qualsiasi applicazione o sito Web in esecuzione sul server Web. Questi temi vengono archiviati per impostazione predefinita nella directory ASP.NETClientfiles-Themes che si trova all'interno della directory v2.x.xxxxx. In alternativa, è possibile spostare i\_file dei\_temi nella cartella aspnet client/system web/[version]/Themes/[theme\_name] nella directory principale del sito Web.

I temi specifici dell'applicazione possono essere applicati solo all'applicazione in cui risiedono i file. Questi file vengono `App\_Themes/<theme\_name>` memorizzati nella directory nella directory principale del sito Web.

## <a name="the-components-of-a-theme"></a>I componenti di un tema

Un tema è costituito da uno o più file CSS, un file skin facoltativo e una cartella Images facoltativa. I file CSS possono essere qualsiasi nome desiderato (ad esempio default.css o theme.css, ecc.) e devono trovarsi nella radice della cartella dei temi. I file CSS vengono utilizzati per definire le classi CSS ordinarie e gli attributi per selettori specifici. Per applicare una delle classi CSS a un elemento della pagina, viene utilizzata la proprietà **CSSClass.**

Il file skin è un file XML che contiene le definizioni delle proprietà per i controlli server ASP.NET. Il codice riportato di seguito è un file skin di esempio.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figura 1** di seguito Mostra una piccola pagina ASP.NET esplorato senza un tema applicato. **Figura 2** Mostra lo stesso file con un tema applicato. Il colore di sfondo e il colore del testo vengono configurati tramite un file CSS. L'aspetto del pulsante e della casella di testo vengono configurati utilizzando il file skin sopra elencato.

![Nessun tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: Nessun tema

![Tema applicato](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: Tema applicato

Il file skin elencato in precedenza definisce un'interfaccia predefinita per tutti i controlli TextBox e Button controlli. Ciò significa che ogni controllo TextBox e Button controllo inserito in una pagina assumerà questo aspetto. È inoltre possibile definire un'interfaccia che può essere applicata a istanze specifiche di questi controlli utilizzando la **SkinID** proprietà del controllo.

Il codice seguente definisce un'interfaccia per un Button controllo. Solo i controlli Button con una proprietà **SkinID** di **goButton** assumeranno l'aspetto dello skin.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

È possibile avere un solo'interfaccia predefinita per ogni tipo di controllo server. Se sono necessarie skin aggiuntive, è necessario utilizzare la proprietà SkinID.

## <a name="applying-themes-to-pages"></a>Applicazione di temi alle pagine

Un tema può essere applicato utilizzando uno dei seguenti metodi:

- &lt;Nell'elemento&gt; pages del file web.config
- Nella @Page direttiva di una pagina
- A livello di codice

## <a name="applying-a-theme-in-the-configuration-file"></a>Applicazione di un tema nel file di configurazione

Per applicare un tema nel file di configurazione delle applicazioni, utilizzare la sintassi seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Il nome del tema specificato qui deve corrispondere al nome della cartella dei temi. Questa cartella può esistere in uno qualsiasi dei percorsi menzionati in precedenza in questo corso. Se si tenta di applicare un tema che non esiste, si verificherà un errore di configurazione.

## <a name="applying-a-theme-in-the-page-directive"></a>Applicazione di un tema nella direttiva Page

È anche possibile applicare un tema nella direttiva di pagina. Questo metodo consente di utilizzare un tema per una pagina specifica.

Per applicare un tema @Page nella direttiva, utilizzare la sintassi seguente:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Ancora una volta, il tema specificato qui deve corrispondere alla cartella del tema come indicato in precedenza. Se si tenta di applicare un tema che non esiste, si verificherà un errore di compilazione. Visual Studio evidenzierà anche l'attributo e notifica che non esiste alcun tema di questo tipo.

## <a name="applying-a-theme-programmatically"></a>Applicazione di un tema a livello di codiceApplying a Theme Programmatically

Per applicare un tema a livello di codice, è necessario specificare la proprietà **Theme** per la pagina nel **metodo Page\_PreInit.**

Per applicare un tema a livello di codice, utilizzare la sintassi seguente:To apply a theme programmatically, use the following syntax:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

È necessario applicare il tema nel metodo PreInit a causa del ciclo di vita della pagina. Se lo si applica dopo tale punto, il tema delle pagine sarà già stato applicato dal runtime e una modifica a quel punto è troppo tardi nel ciclo di vita. Se si applica un tema che non esiste, si verifica **un'eccezione HttpException.If** you apply a theme that doesn't exist, an HttpException occurs. Quando un tema viene applicato a livello di codice, si verificherà un avviso di compilazione se per tutti i controlli server è specificata una proprietà SkinID. Questo avviso ha lo scopo di informare l'utente che nessun tema viene applicato in modo dichiarativo e può essere ignorato.

## <a name="exercise-1--applying-a-theme"></a>Esercizio 1 : Applicazione di un tema

In questo esercizio verrà applicata una ASP.NET tema a un sito Web.

> [!IMPORTANT]
> Se si utilizza Microsoft Word per immettere informazioni in un file skin, assicurarsi di non sostituire le virgolette normali tra virgolette inglesi. Le citazioni intelligenti causeranno problemi con i file di skin.

1. Creare un nuovo sito Web ASP.NET.
2. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
3. Scegliere File di configurazione Web dall'elenco dei file e fare clic su Aggiungi.
4. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
5. Selezionate File di skin (Skin File) e fate clic su Aggiungi (Add).
6. Fare clic su Sì quando viene chiesto se si\_desidera inserire il file all'interno della cartella Temi dell'app.
7. Fare clic con il pulsante destro\_del mouse sulla cartella SkinFile all'interno della cartella Temi app in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
8. Scegliere Foglio di stile dall'elenco dei file e fare clic su Aggiungi. Ora hai tutti i file necessari per implementare il tuo nuovo tema. Tuttavia, Visual Studio ha denominato la cartella dei temi SkinFile.However, Visual Studio has named your themes folder SkinFile. Fare clic con il pulsante destro del mouse su tale cartella e modificare il nome in CoolTheme.
9. Aprire il file SkinFile.skin e aggiungere il codice seguente alla fine del file: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Salvate il file SkinFile.skin.
11. Aprire il file StyleSheet.css.
12. Sostituire tutto il testo in esso contenuto con quanto segue: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Salvare il file StyleSheet.css.
14. Aprire la pagina Default.aspx.
15. Aggiungere un controllo TextBox e un controllo Button.Add a TextBox control and a Button control.
16. Salvare la pagina. A questo punto esplorare la pagina Default.aspx. Dovrebbe essere visualizzato come un normale modulo Web.
17. Aprire il file Web. config.
18. Aggiungere quanto segue direttamente `<system.web>` sotto il tag di apertura: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Salvare il file web.config. A questo punto esplorare la pagina Default.aspx. Dovrebbe essere visualizzato con il tema applicato.
20. Se non è già aperto, aprire la pagina Default.aspx in Visual Studio.If it's not already open, open the Default.aspx page in Visual Studio.
21. Selezionare il pulsante .
22. Modificare la proprietà **SkinID** in goButton. Si noti che Visual Studio fornisce un elenco a discesa con valori SkinID validi per un controllo Button.Notice that Visual Studio provides a dropdown with valid SkinID values for a Button control.
23. Salvare la pagina. Ora visualizza di nuovo l'anteprima della pagina nel tuo browser. Il pulsante dovrebbe ora dire "vai" e dovrebbe essere più ampio nell'aspetto.

Utilizzando la proprietà **SkinID,** è possibile configurare facilmente interfacce diverse per istanze diverse di un particolare tipo di controllo server.

## <a name="the-stylesheettheme-property"></a>La proprietà StyleSheetTheme

Finora, abbiamo parlato solo di applicare temi utilizzando la proprietà Theme. Quando si utilizza la proprietà Theme, il file di interfaccia eseguirà l'override di tutte le impostazioni dichiarative per i controlli server. Ad esempio, nell'esercizio 1 è stato specificato uno SkinID di "goButton" per il controllo Button e che ha modificato il testo del pulsante in "go". Avrete notato che il Text proprietà del Button nella finestra di progettazione è stata impostata su "Button", ma il tema ha superato che. Il tema eseguirà sempre l'override di tutte le impostazioni delle proprietà nella finestra di progettazione.

Se si desidera essere in grado di eseguire l'override delle proprietà definite nel file di interfaccia del tema con le proprietà specificate nella finestra di progettazione, è possibile utilizzare il **StyleSheetTheme** proprietà anziché il Theme proprietà. Il StyleSheetTheme proprietà è uguale al Theme proprietà ad eccezione del fatto che non esegue l'override di tutte le impostazioni di proprietà esplicite come il Theme proprietà.

Per visualizzare questa operazione, aprire il file web.config dal `<pages>` progetto nell'esercizio 1 e modificare l'elemento nel modo seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

A questo punto esplorare il Default.aspx pagina e si noterà che il Button controllo ha un Text proprietà di "Button" nuovamente. Questo perché l'impostazione esplicita della proprietà nella finestra di progettazione esegue l'override di Text proprietà impostata dal goButton SkinID.

## <a name="overriding-themes"></a>Override dei temi

Un tema globale può essere sostituito applicando un tema con\_lo stesso nome nella cartella Temi app dell'applicazione. Tuttavia, il tema non viene applicato in uno scenario di override true. Se il runtime rileva i file\_del tema nella cartella Temi dell'app, applicherà il tema utilizzando tali file e ignorerà il tema globale.

Il StyleSheetTheme proprietà viene sottoponibile a override e può essere sottoposto a override nel codice come segue:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>web part

ASP.NET Web part è un insieme integrato di controlli per la creazione di siti Web che consentono agli utenti finali di modificare il contenuto, l'aspetto e il comportamento delle pagine Web direttamente da un browser. Le modifiche possono essere applicate a tutti gli utenti del sito o a singoli utenti. Quando gli utenti modificano pagine e controlli, le impostazioni possono essere salvate per mantenere le preferenze personali dell'utente nelle future sessioni del browser, una funzionalità denominata personalizzazione. Queste funzionalità Web part consentono agli sviluppatori di consentire agli utenti finali di personalizzare un'applicazione Web in modo dinamico, senza l'intervento di sviluppatori o amministratori.

Utilizzando il set di controlli Web part, gli utenti finali possono consentire agli utenti finali di:

- Personalizzare il contenuto della pagina. Gli utenti possono aggiungere nuovi controlli Web part a una pagina, rimuoverli, nasconderli o ridurli a icona come normali finestre.
- Personalizzare il layout di pagina. Gli utenti possono trascinare un controllo Web part in un'area diversa di una pagina o modificarne l'aspetto, le proprietà e il comportamento.
- Controlli di esportazione e importazione. Gli utenti possono importare o esportare le impostazioni dei controlli Web part per l'utilizzo in altre pagine o siti, mantenendo le proprietà, l'aspetto e persino i dati nei controlli. In questo modo si riducono le richieste di immissione e configurazione dei dati per gli utenti finali.
- Creare connessioni. Gli utenti possono stabilire connessioni tra i controlli in modo che, ad esempio, un controllo grafico possa visualizzare un grafico per i dati in un controllo ticker azionario. Gli utenti possono personalizzare non solo la connessione stessa, ma anche l'aspetto e i dettagli della modalità di visualizzazione dei dati da parte del controllo grafico.
- Gestire e personalizzare le impostazioni a livello di sito. Gli utenti autorizzati possono configurare le impostazioni a livello di sito, determinare chi può accedere a un sito o a una pagina, impostare l'accesso basato sui ruoli ai controlli e così via. Ad esempio, un utente con un ruolo amministrativo può impostare un controllo Web part per essere condiviso da tutti gli utenti e impedire agli utenti che non sono amministratori di personalizzare il controllo condiviso.

Le web part vengono in genere utilizzate in uno dei tre modi seguenti: creazione di pagine che utilizzano controlli Web part, creazione di singoli controlli Web part o creazione di applicazioni Web complete e personalizzabili, ad esempio un portale.

## <a name="page-development"></a>Sviluppo delle pagine

Gli sviluppatori di pagine possono utilizzare strumenti di progettazione visiva come Microsoft Visual Studio 2005 per creare pagine che utilizzano web part. Uno dei vantaggi nell'utilizzo di uno strumento come Visual Studio è che il set di controlli Web part fornisce funzionalità per la creazione di trascinamento e la configurazione di controlli Web part in una finestra di progettazione visiva. Ad esempio, è possibile utilizzare la finestra di progettazione per trascinare un'area Web part o un controllo editor Web part nell'area di progettazione e quindi configurare il controllo direttamente nella finestra di progettazione utilizzando l'interfaccia utente fornita dal set di controlli Web part. Ciò può velocizzare lo sviluppo di applicazioni Web part e ridurre la quantità di codice da scrivere.

## <a name="control-development"></a>Sviluppo dei controlli

È possibile utilizzare qualsiasi controllo ASP.NET esistente come controllo Web part, inclusi i controlli server Web standard, i controlli server personalizzati e i controlli utente. Per il massimo controllo a livello di codice dell'ambiente, è inoltre possibile creare controlli Web part personalizzati che derivano dalla WebPart classe. Per lo sviluppo di singoli controlli Web part, in genere si crea un controllo utente e lo si utilizza come controllo Web part oppure si sviluppa un controllo Web part personalizzato.

Come esempio di sviluppo di un controllo Web part personalizzato, è possibile creare un controllo per fornire una qualsiasi delle funzionalità fornite da altri controlli server ASP.NET che potrebbero essere utili per il pacchetto come controllo Web part personalizzabile: calendari, elenchi, informazioni finanziarie, notizie, calcolatrici, controlli RTF per l'aggiornamento del contenuto, griglie modificabili che si connettono ai database, grafici che aggiornano dinamicamente i propri display , o informazioni meteo e di viaggio. Se si fornisce una finestra di progettazione visiva con il controllo, qualsiasi sviluppatore di pagine che utilizza Visual Studio può semplicemente trascinare il controllo in una zona Web part e configurarlo in fase di progettazione senza dover scrivere codice aggiuntivo.

La personalizzazione è alla base della funzionalità Web part. Consente agli utenti di modificare o personalizzare il layout, l'aspetto e il comportamento dei controlli Web part in una pagina. Le impostazioni personalizzate sono di lunga durata: vengono mantenute non solo durante la sessione corrente del browser (come per lo stato di visualizzazione), ma anche nell'archiviazione a lungo termine, in modo che le impostazioni di un utente vengano salvate anche per le sessioni future del browser. La personalizzazione è abilitata per impostazione predefinita per le pagine web part.

I componenti strutturali dell'interfaccia utente si basano sulla personalizzazione e forniscono la struttura di base e i servizi necessari per tutti i controlli Web part. Un componente strutturale dell'interfaccia utente richiesto in ogni pagina Web part è il controllo WebPartManager.One UI structural component required on every Web Parts page is the WebPartManager control. Sebbene non sia mai visibile, questo controllo ha l'attività critica di coordinare tutti i controlli Web part in una pagina. Ad esempio, tiene traccia di tutti i singoli controlli Web part. Gestisce le aree Web part (aree che contengono controlli Web part in una pagina) e quali controlli sono in quali aree. Tiene inoltre traccia e controlla le diverse modalità di visualizzazione in cui può trovarsi una pagina, ad esempio la modalità di esplorazione, connessione, modifica o catalogo e se le modifiche di personalizzazione si applicano a tutti gli utenti o a singoli utenti. Infine, avvia e tiene traccia delle connessioni e delle comunicazioni tra i controlli Web part.

Il secondo tipo di componente strutturale dell'interfaccia utente è la zona. Le aree fungono da gestori di layout in una pagina Web part. Contengono e organizzano i controlli che derivano dalla classe Part (controlli parte) e offrono la possibilità di eseguire il layout di pagina modulare con orientamento orizzontale o verticale. Le aree offrono anche elementi dell'interfaccia utente comuni e coerenti (ad esempio lo stile dell'intestazione e del piè di pagina, il titolo, lo stile del bordo, i pulsanti di azione e così via) per ogni controllo che contengono; questi elementi comuni sono noti come il cromo di un controllo. Diversi tipi specializzati di zone vengono utilizzati nelle diverse modalità di visualizzazione e con vari controlli. I diversi tipi di aree sono descritti nella sezione Controlli essenziali Web part riportata di seguito.

I controlli dell'interfaccia utente Web part, che derivano tutti dalla classe **Part,** costituiscono l'interfaccia utente principale in una pagina Web part. Il set di controlli Web part è flessibile e inclusivo nelle opzioni disponibili per la creazione di controlli di parte. Oltre a creare controlli Web part personalizzati, è anche possibile utilizzare i controlli server ASP.NET esistenti, i controlli utente o i controlli server personalizzati come controlli Web part. I controlli essenziali più comunemente utilizzati per la creazione di pagine web part sono descritti nella sezione successiva.

## <a name="web-parts-essential-controls"></a>Controlli essenziali delle web part

Il set di controlli Web part è esteso, ma alcuni controlli sono essenziali perché sono necessari per il funzionamento delle web part o perché sono i controlli utilizzati più di frequente nelle pagine web part. Quando si inizia a utilizzare le web part e a creare pagine Web part di base, è utile acquisire familiarità con i controlli Web part essenziali descritti nella tabella seguente.

| **controllo Web part** | **Descrizione** |
| --- | --- |
| Webpartmanager | Gestisce tutti i controlli Web part in una pagina. Per ogni pagina Web part è necessario un (e un solo) controllo **WebPartManager.** |
| Catalogzone | Contiene i controlli CatalogPart. Utilizzare questa area per creare un catalogo di controlli Web part da cui gli utenti possono selezionare i controlli da aggiungere a una pagina. |
| Editorzone | Contiene i controlli EditorPart. Utilizzare questa area per consentire agli utenti di modificare e personalizzare i controlli Web part in una pagina. |
| Webpartzone | Contiene e fornisce il layout complessivo per il WebPart controlli che compongono l'interfaccia utente principale di una pagina. Utilizzare questa area ogni volta che si creano pagine con controlli Web part. Le pagine possono contenere una o più aree. |
| Connectionszone | Contiene i controlli WebPartConnection e fornisce un'interfaccia utente per la gestione delle connessioni. |
| WebPart (GenericWebPart) | Esegue il rendering dell'interfaccia utente principale; la maggior parte dei controlli dell'interfaccia utente Web part rientrano in questa categoria. Per il massimo controllo a livello di codice, è possibile creare controlli Web part personalizzati che derivano dal base **WebPart** controllo. È inoltre possibile utilizzare i controlli server esistenti, i controlli utente o i controlli personalizzati come controlli Web part. Ogni volta che uno di questi controlli vengono inseriti in una zona, il **WebPartManager** controllo automaticamente esegue il wrapping con **GenericWebPart** controlli in fase di esecuzione in modo che è possibile utilizzarli con la funzionalità Web part. |
| Catalogpart | Contiene un elenco di controlli Web part disponibili che gli utenti possono aggiungere alla pagina. |
| Webpartconnection | Crea una connessione tra due controlli Web part in una pagina. La connessione definisce uno dei controlli Web part come provider (di dati) e l'altro come consumer. |
| Editorpart | Funge da classe base per i controlli editor specializzati. |
| Controlli EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart e PropertyGridEditorPart) | Consentire agli utenti di personalizzare vari aspetti dei controlli dell'interfaccia utente Web part in una pagina |

## <a name="lab-create-a-web-part-page"></a>Lab: Creare una pagina web part

In questa esercitazione verrà creata una pagina Web part che renderà persistenti le informazioni tramite profili di ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Creazione di una pagina semplice con web part

In questa parte della procedura dettagliata viene creata una pagina che utilizza controlli Web part per visualizzare contenuto statico. Il primo passaggio nell'utilizzo delle web part consiste nel creare una pagina con due elementi strutturali necessari. In primo luogo, una pagina Web part richiede un WebPartManager controllo per tenere traccia e coordinare tutti i controlli Web part. In secondo luogo, una pagina Web part richiede una o più aree, ovvero controlli compositi che contengono WebPart o altri controlli server e occupano un'area specificata di una pagina.

> [!NOTE]
> Non è necessario eseguire alcuna operazione per abilitare la personalizzazione di Web part; è abilitato per impostazione predefinita per il set di controlli Web part. Quando si esegue per la prima volta una pagina Web part in un sito, ASP.NET configura un provider di personalizzazioni predefinito per archiviare le impostazioni di personalizzazione utente. Per ulteriori informazioni sulla personalizzazione, vedere Cenni preliminari sulla personalizzazione di Web part.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Per creare una pagina per contenere controlli Web part

1. Chiudere la pagina predefinita e aggiungere una nuova pagina al sito denominato WebPartsDemo.aspx.
2. Passare alla visualizzazione **Progettazione.**
3. Dal menu **Visualizza,** assicurarsi che le opzioni **Controlli non visivi** e **Dettagli** siano selezionate in modo da poter visualizzare i tag di layout e i controlli che non dispongono di un'interfaccia utente.
4. Posizionare il punto `<div>` di inserimento prima dei tag nell'area di progettazione e premere INVIO per aggiungere una nuova riga. Posizionare il punto di inserimento prima del carattere della nuova riga, fare clic sul controllo elenco a discesa **Formato** blocco nel menu e selezionare l'opzione **Titolo 1.** Nell'intestazione aggiungere il testo **Pagina dimostrativa web part**.
5. Dalla scheda **WebParts** della Casella degli strumenti trascinare un controllo **WebPartManager** nella pagina, posizionandolo subito dopo il carattere di nuova riga e prima dei `<div>`tag.   
  
   Il **WebPartManager** controllo non esegue il rendering di alcun output, pertanto viene visualizzato come una casella grigia nell'area di progettazione.
6. Posizionare il punto `<div>` di inserimento all'interno dei tag.
7. Scegliere **Inserisci tabella**dal menu **Layout** e creare una nuova tabella con una riga e tre colonne. Fare clic sul pulsante **Proprietà cella,** selezionare **top** dall'elenco a discesa **Allineamento verticale,** fare clic su **OK**e fare di nuovo clic **su OK** per creare la tabella.
8. Trascinare un controllo WebPart zoone nella colonna sinistra della tabella. Fare clic con il pulsante destro del mouse sul controllo **WebPart zoone,** scegliere **Proprietà**e impostare le proprietà seguenti:   
  
   ID: Sidebar-one   
  
   HeaderText: Intestazione laterale
9. Trascinare un secondo controllo **WebPart zoone** nella colonna centrale della tabella e impostare le seguenti proprietà:   
  
   ID: Main zoone   
  
   HeaderText: Principale
10. Salvare il file.

La pagina dispone ora di due aree distinte che è possibile controllare separatamente. Tuttavia, nessuna delle due aree ha alcun contenuto, quindi la creazione di contenuto è il passaggio successivo. Per questa procedura dettagliata, si utilizzano controlli Web part che visualizzano solo contenuto statico.

Il layout di una zona Web &lt;part&gt; è specificato da un elemento zonetemplate. All'interno del modello di zona, è possibile aggiungere qualsiasi controllo ASP.NET, che si tratti di un controllo Web part personalizzato, di un controllo utente o di un controllo server esistente. Si noti che in questo caso si utilizza il Label controllo e a che si sta semplicemente aggiungendo testo statico. Quando si inserisce un normale controllo server in una zona **WebPart,** ASP.NET considera il controllo come un controllo Web part in fase di esecuzione, che abilita le funzionalità Web part nel controllo.

**Per creare contenuto per l'area principale**

1. Nella visualizzazione **Progettazione** trascinare un controllo **Label** dalla scheda **Standard** della Casella degli strumenti nell'area del contenuto dell'area la cui proprietà **ID** è impostata su Main.
2. Passare alla visualizzazione **Origine.** Si noti che è stato aggiunto un &lt;elemento zonetemplate&gt; per eseguire il wrapping del controllo **Label** nel controllo Main.
3. Aggiungere un **title** attributo &lt;denominato title&gt; all'elemento asp:label e impostarne il valore su Content. Rimuovere l'attributo Text "Label" &lt;&gt; dall'elemento asp:label. Tra i tag di &lt;apertura e&gt; chiusura dell'elemento asp:label aggiungere del &lt;testo,&gt; ad esempio Welcome to my **Home Page,** all'interno di una coppia di tag di elementi h2. Il codice dovrebbe avere l'aspetto seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Salvare il file.

Successivamente, creare un controllo utente che può essere aggiunto alla pagina come controllo Web part.

### <a name="to-create-a-user-control"></a>Per creare un controllo utente

1. Aggiungere un nuovo controllo utente Web al sito da utilizzare come controllo di ricerca. Deselezionare l'opzione **Inserisci codice sorgente in un file separato.** Aggiungerlo nella stessa directory della pagina WebPartsDemo.aspx e denominarlo SearchUserControl.ascx.   
  
    > [!NOTE]
    > Il controllo utente per questa procedura dettagliata non implementa la funzionalità di ricerca effettiva; viene utilizzato solo per illustrare le funzionalità Web part.
2. Passare alla visualizzazione **Progettazione.** Dalla scheda **Standard** della Casella degli strumenti trascinare un controllo TextBox nella pagina.
3. Posizionare il punto di inserimento dopo la casella di testo appena aggiunta e premere INVIO per aggiungere una nuova riga.
4. Trascinare un controllo Button nella pagina nella nuova riga sotto la casella di testo appena aggiunta.
5. Passare alla visualizzazione **Origine.** Verificare che il codice sorgente per il controllo utente sia simile all'esempio seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Salvare e chiudere il file.

A questo punto è possibile aggiungere controlli Web part all'area dell'area dell'area dell'area dell'area dell'area. Si stanno aggiungendo due controlli all'area dell'armatura laterale, uno contenente un elenco di collegamenti e un altro che è il controllo utente creato nella procedura precedente. I collegamenti vengono aggiunti come controllo server **Label** standard, in modo simile al modo in cui è stato creato il testo statico per la zona principale. Tuttavia, anche se i singoli controlli server contenuti nel controllo utente potrebbero essere contenuti direttamente nella zona (come il controllo etichetta), in questo caso non lo sono. Al contrario, fanno parte del controllo utente creato nella procedura precedente. Viene illustrato un modo comune per creare un pacchetto di controlli e funzionalità aggiuntive che si desidera in un controllo utente e quindi fare riferimento a tale controllo in una zona come controllo Web part.

In fase di esecuzione, il set di controlli Web part esegue il wrapping di entrambi i controlli con i controlli GenericWebPart . Quando un controllo **GenericWebPart** esegue il wrapping di un controllo server Web, il controllo parte generico è il controllo padre ed è possibile accedere al controllo server tramite la proprietà ChildControl del controllo padre. Questo utilizzo di controlli di parte generici consente ai controlli server Web standard di avere lo stesso comportamento di base e gli stessi attributi dei controlli Web part che derivano dalla classe **WebPart.**

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Per aggiungere controlli Web part all'area dell'area dell'area dell'area dell'area dell'area

1. Aprire la pagina WebPartsDemo.aspx.
2. Passare alla visualizzazione **Progettazione.**
3. Trascinare la pagina del controllo utente creata, SearchUserControl.ascx, da **Esplora soluzioni** nella zona la cui proprietà **ID** è impostata su Sidebar-one e rilasciarla in questo campo.
4. Salvare la pagina WebPartsDemo.aspx.
5. Passare alla visualizzazione **Origine.**
6. All'interno &lt;dell'elemento&gt; asp:webpartzone per la barra laterale, appena &lt;sopra&gt; il riferimento al controllo utente, aggiungere un elemento asp:label con collegamenti contenuti, come illustrato nell'esempio seguente. Aggiungere inoltre un attributo **Title** al tag del controllo utente, con un valore **Search**, come illustrato. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Salvare e chiudere il file.

Ora puoi testare la tua pagina sfogliandola nel tuo browser. Nella pagina vengono visualizzate le due aree. Nella cattura di schermata seguente viene illustrata la pagina.

**Pagina dimostrativa web part con due aree**

![Schermata di Web part VS 1](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: Web part VS Procedura dettagliata 1 Schermata

Nella barra del titolo di ogni controllo è una freccia verso il basso che fornisce l'accesso a un menu di verbi di azioni disponibili che è possibile eseguire su un controllo. Fare clic sul menu dei verbi per uno dei controlli, quindi fare clic sul verbo **Riduci a** icona e notare che il controllo è ridotto a icona. Scegliere **Ripristina**dal menu verbi e il controllo torna alle dimensioni normali.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Abilitazione degli utenti alla modifica delle pagine e alla modifica del layout

Le web part offrono agli utenti la possibilità di modificare il layout dei controlli Web part trascinandoli da un'area all'altra. Oltre a consentire agli utenti di spostare **WebPart** controlli da un'area all'altra, è possibile consentire agli utenti di modificare varie caratteristiche dei controlli, tra cui il loro aspetto, layout e comportamento. Il set di controlli Web part fornisce funzionalità di modifica di base per i controlli **WebPart.** Anche se non sarà necessario eseguire questa operazione in questa procedura dettagliata, è anche possibile creare controlli editor personalizzati che consentono agli utenti di modificare le funzionalità di **WebPart** controlli. Come per la modifica della posizione di un **WebPart** controllo, la modifica delle proprietà di un controllo si basa su ASP.NET personalizzazione per salvare le modifiche apportate dagli utenti.

In questa parte della procedura dettagliata, si aggiunge la possibilità per gli utenti di modificare le caratteristiche di base di qualsiasi **WebPart** controllo nella pagina. Per abilitare queste funzionalità, aggiungere un altro controllo &lt;utente personalizzato&gt; alla pagina, insieme a un elemento asp:editorzone e due controlli di modifica.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Per creare un controllo utente che consenta la modifica del layout di paginaTo create a user control that enables changing page layout

1. In Visual Studio scegliere il sottomenu **Nuovo** dal menu **File,** quindi fare clic sull'opzione **File.**
2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare Controllo utente **Web**. Assegnare al nuovo file il nome DisplayModeMenu.ascx. Deselezionare l'opzione **Inserisci codice sorgente in un file separato.**
3. Fare clic su Aggiungi per creare il nuovo controllo.
4. Passare alla visualizzazione **Origine.**
5. Rimuovere tutto il codice esistente nel nuovo file e incollare il codice seguente. Questo codice di controllo utente utilizza funzionalità del set di controlli Web part che consentono a una pagina di modificarne la visualizzazione o la modalità di visualizzazione e consente inoltre di modificare l'aspetto fisico e il layout della pagina mentre si è in determinate modalità di visualizzazione. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Salvare il file facendo clic sull'icona Salva sulla barra degli strumenti oppure scegliendo **Salva** dal menu **File.**

### <a name="to-enable-users-to-change-the-layout"></a>Per consentire agli utenti di modificare il layout

1. Aprire la pagina WebPartsDemo.aspx e passare alla visualizzazione **Progettazione.**
2. Posizionare il punto di inserimento nella visualizzazione **Progettazione** subito dopo il controllo **WebPartManager** aggiunto in precedenza. Aggiungere un ritorno a capo dopo il testo in modo che vi sia una riga vuota dopo il **WebPartManager** controllo. Posizionare il punto di inserimento sulla riga vuota.
3. Trascinare il controllo utente appena creato (il file è denominato DisplayModeMenu.ascx) nella pagina WebPartsDemo.aspx e rilasciarlo sulla riga vuota.
4. Trascinare un controllo Editor-one dalla sezione **WebParts** della Casella degli strumenti alla cella di tabella aperta rimanente nella pagina WebPartsDemo.aspx.
5. Dalla sezione **WebParts** della Casella degli strumenti trascinare un controllo AppearanceEditorPart e un controllo LayoutEditorPart nel controllo **Editor.one.**
6. Passare alla visualizzazione **Origine.** Il codice risultante nella cella della tabella dovrebbe essere simile al codice seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Salvare il file WebPartsDemo.aspx. È stato creato un controllo utente che consente di modificare le modalità di visualizzazione e il layout di pagina e si è fatto riferimento al controllo nella pagina Web principale.

È ora possibile testare la funzionalità per modificare le pagine e modificare il layout.

### <a name="to-test-layout-changes"></a>Per testare le modifiche al layout

1. Caricare la pagina in un browser.
2. Fare clic sul menu a discesa **Modalità** di visualizzazione e selezionare **Modifica**. Vengono visualizzati i titoli delle zone.
3. Trascinare il controllo **Collegamenti personali** dalla barra del titolo dall'area dell'area dell'ombreggiatura alla parte inferiore dell'area principale. La pagina dovrebbe essere simile alla schermata seguente.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Pagina dimostrativa web part con il controllo Collegamenti personali spostato

![Schermata di Web part VS 2](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: Web part VS Procedura dettagliata 2 Schermata

1. Fare clic sul menu a discesa **Modalità** di visualizzazione e selezionare **Sfoglia**. La pagina viene aggiornata, i nomi delle zone scompaiono e il controllo **Collegamenti personali** rimane nella posizione in cui è stata posizionata.
2. Per dimostrare che la personalizzazione funziona, chiudere il browser e quindi caricare nuovamente la pagina. Le modifiche apportate vengono salvate per le sessioni future del browser.
3. Dal menu **Modalità di visualizzazione,** selezionare **Modifica**.   
  
   Ogni controllo nella pagina viene ora visualizzato con una freccia verso il basso nella barra del titolo, che contiene il menu a discesa dei verbi.
4. Fare clic sulla freccia per visualizzare il menu dei verbi nel controllo **Collegamenti** personali. Fare clic sul verbo **Modifica.**   
  
   Viene visualizzato il controllo **Editor-one,** che visualizza i controlli EditorPart aggiunti.
5. Nella sezione **Aspetto** del controllo di modifica modificare il **titolo** in Preferiti, utilizzare l'elenco a discesa **Tipo di Chrome** per selezionare Solo **titolo**e quindi fare clic su **Applica**. Nell'immagine riportata di seguito viene illustrata la pagina in modalità di modifica.

### <a name="web-parts-demo-page-in-edit-mode"></a>Pagina Demo web part in modalità di modifica

![Schermata di Web part VS 3](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: Web part VS Procedura dettagliata 3 Schermata

1. Fare clic sul menu **Display Mode (Modalità di visualizzazione)** e selezionare **Browse (Sfoglia)** per tornare alla modalità browse.
2. Il controllo ha ora un titolo aggiornato e nessun bordo, come illustrato nella schermata seguente.

### <a name="edited-web-parts-demo-page"></a>Pagina dimostrativa web part modificate

![Schermata della procedura dettagliata di Web part VS 4](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: Web part VS Procedura dettagliata 4 Schermata

### <a name="adding-web-parts-at-run-time"></a>Aggiunta di web part in fase di esecuzione

È inoltre possibile consentire agli utenti di aggiungere controlli Web part alla pagina in fase di esecuzione. A tale scopo, configurare la pagina con un catalogo Web part, che contiene un elenco di controlli Web part che si desidera rendere disponibili agli utenti.

**Per consentire agli utenti di aggiungere web part in fase di esecuzione**

1. Aprire la pagina WebPartsDemo.aspx e passare alla visualizzazione **Progettazione.**
2. Dalla scheda **WebParts** della Casella degli strumenti, trascinare un controllo Catalogzone nella colonna destra della tabella, sotto il controllo **Editor.**   
  
   Entrambi i controlli possono trovarsi nella stessa cella di tabella perché non verranno visualizzati contemporaneamente.
3. Nel riquadro Proprietà assegnare la stringa **Aggiungi web part** alla proprietà HeaderText del controllo **Catalog.**
4. Dalla sezione **WebParts** della Casella degli strumenti trascinare un controllo DeclarativeCatalogPart nell'area di contenuto del controllo **Catalog.**
5. Fare clic sulla freccia nell'angolo superiore destro del controllo **DeclarativeCatalogPart** per visualizzare il relativo menu Attività, quindi selezionare **Modifica modelli**.
6. Dalla sezione **Standard** della Casella degli strumenti trascinare un controllo **FileUpload** e un controllo **Calendar** nella sezione **WebPartsTemplate** del controllo **DeclarativeCatalogPart.**
7. Passare alla visualizzazione **Origine.** Esaminare il codice &lt;sorgente dell'elemento asp:catalogzone.&gt; Si noti che il **DeclarativeCatalogPart** controllo contiene un &lt;webpartstemplate&gt; elemento con i due controlli server inclusi che sarà possibile aggiungere alla pagina dal catalogo.
8. Aggiungere una proprietà **Title** a ognuno dei controlli aggiunti al catalogo, usando il valore stringa visualizzato per ogni titolo nell'esempio di codice riportato di seguito. Anche se il titolo non è una proprietà è in genere possibile impostare su questi due controlli server in fase di progettazione, quando un utente aggiunge questi controlli a **un'area WebPart zone** dal catalogo in fase di esecuzione, ognuno viene eseguito il wrapping con un **GenericWebPart** controllo. In questo modo possono fungere da controlli Web part, in modo che possano visualizzare i titoli.   
  
   Il codice per i due controlli contenuti nel **DeclarativeCatalogPart** controllo deve essere simile al seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Salvare la pagina.

È ora possibile testare il catalogo.

### <a name="to-test-the-web-parts-catalog"></a>Per testare il catalogo web part

1. Caricare la pagina in un browser.
2. Fare clic sul menu a discesa **Modalità** di visualizzazione e selezionare **Catalogo**.   
  
   Viene visualizzato il catalogo denominato **Aggiungi web part.**
3. Trascinare il controllo **Preferiti** dall'area principale nella parte superiore dell'area dell'area dell'area dell'area dell'area dell'area dell'area dell'area dell'area dell'area dell'area dell'area.
4. Nel catalogo **Aggiungi web part** selezionare entrambe le caselle di controllo e quindi selezionare **Principale** dall'elenco a discesa contenente le aree disponibili.
5. Fare clic su **Aggiungi** nel catalogo. I controlli vengono aggiunti alla zona principale. Se lo si desidera, è possibile aggiungere più istanze di controlli dal catalogo alla pagina.   
  
   Nella cattura di schermata seguente viene illustrata la pagina con il controllo di caricamento file e il calendario nell'area principale. 

![Controlli aggiunti alla zona principale dal catalogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Fare clic sul menu a discesa **Modalità** di visualizzazione e selezionare **Sfoglia**. Il catalogo scompare e la pagina viene aggiornata.
7. Chiudere il browser. Caricare nuovamente la pagina. Le modifiche apportate vengono mantenute.
