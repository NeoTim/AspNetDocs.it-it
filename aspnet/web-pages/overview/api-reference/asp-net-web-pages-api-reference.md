---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Guida di riferimento rapido all'API ASP.NET (Razor) Documenti Microsoft
author: Rick-Anderson
description: Questa pagina contiene un elenco con brevi esempi degli oggetti, delle proprietà e dei metodi più comunemente utilizzati per la programmazione di pagine Web ASP.NET con sintassi Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676244"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>Guida di riferimento rapido dell'API ASP.NET (Razor)

 di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa pagina contiene un elenco con brevi esempi degli oggetti, delle proprietà e dei metodi più comunemente utilizzati per la programmazione di pagine Web ASP.NET con sintassi Razor.
> 
> Le descrizioni contrassegnate con "(v2)" sono state introdotte in ASP.NET pagine Web versione 2.
> 
> Per la documentazione di riferimento per le API, vedere la [documentazione di riferimento](https://go.microsoft.com/fwlink/?LinkId=208659) di ASP.NET Web Pages su MSDN.
> 
> ## <a name="software-versions"></a>Versioni software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con ASP.NET Pagine Web 2 e pagine Web ASP.NET 1.0 (ad eccezione delle funzionalità contrassegnate v2).

Questa pagina contiene informazioni di riferimento per quanto segue:

- [Classi](#Classes)
- [Dati](#Data)
- [Helper](#Helpers)
- [Validation](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Classi

### `AppState[key], AppState[index],App`

Contiene dati che possono essere condivisi da qualsiasi pagina dell'applicazione. È possibile utilizzare `App` la proprietà dynamic per accedere agli stessi dati, come nell'esempio seguente:You can use the dynamic property to access the same data, as in the following example:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Converte un valore stringa in un valore booleano (true/false). Restituisce false o il valore specificato se la stringa non rappresenta true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Converte un valore stringa in data/ora. Restituisce `DateTime.MinValue` o il valore specificato se la stringa non rappresenta una data/ora.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Converte un valore stringa in un valore decimale. Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Converte un valore stringa in un valore float. Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Converte un valore stringa in un numero intero. Restituisce 0 o il valore specificato se la stringa non rappresenta un numero intero.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crea un URL compatibile con il browser da un percorso di file locale, con parti di percorso aggiuntive facoltative.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Esegue il *rendering del valore* come markup HTML anziché eseguirne il rendering come output con codifica HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Restituisce true se il valore può essere convertito da una stringa al tipo specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Restituisce true se l'oggetto o la variabile non ha alcun valore.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Restituisce true se la richiesta è un POST. (Le richieste iniziali sono in genere un GET.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Specifica il percorso di una pagina di layout da applicare a questa pagina.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contiene i dati condivisi tra la pagina, le pagine di layout e le pagine parziali nella richiesta corrente. È possibile utilizzare `Page` la proprietà dynamic per accedere agli stessi dati, come nell'esempio seguente:You can use the dynamic property to access the same data, as in the following example:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Pagine di layout) Esegue il rendering del contenuto di una pagina di contenuto che non è presente in alcuna sezione denominata.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Esegue il rendering di una pagina di contenuto utilizzando il percorso specificato e dati aggiuntivi facoltativi. È possibile ottenere i valori `PageData` dei parametri aggiuntivi da posizione (esempio 1) o chiave (esempio 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Pagine di layout) Esegue il rendering di una sezione di contenuto con un nome. Impostare *required* su false per rendere facoltativa una sezione.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Ottiene o imposta il valore di un cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Ottiene i file caricati nella richiesta corrente.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ottiene i dati pubblicati in un form (come stringhe). `Request[key]`controlla sia `Request.Form` la `Request.QueryString` raccolta che la raccolta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Ottiene i dati specificati nella stringa di query dell'URL. `Request[key]`controlla sia `Request.Form` la `Request.QueryString` raccolta che la raccolta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Disabilita in modo selettivo la convalida della richiesta per un elemento del modulo, un valore della stringa di query, un cookie o un valore di intestazione. La convalida delle richieste è abilitata per impostazione predefinita e impedisce agli utenti di pubblicare markup o altri contenuti potenzialmente pericolosi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Aggiunge un'intestazione del server HTTP alla risposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Memorizza nella cache l'output della pagina per un periodo di tempo specificato. Facoltativamente, impostare *lo scorrimento* per reimpostare il timeout in ogni pagina di accesso e *varyByParams* per memorizzare nella cache versioni diverse della pagina per ogni stringa di query diversa nella richiesta di pagina.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Reindirizza la richiesta del browser a una nuova posizione.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Imposta il codice di stato HTTP inviato al browser.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Scrive il contenuto dei *dati* nella risposta con un tipo MIME facoltativo.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Scrive il contenuto di un file nella risposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Pagine di layout) Definisce una sezione di contenuto con un nome.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodifica una stringa codificata in HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codifica una stringa per il rendering nel markup HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Restituisce il percorso fisico del server per il percorso virtuale specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodifica il testo da un URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codifica il testo da inserire in un URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Ottiene o imposta un valore esistente fino alla chiusura del browser da parte dell'utente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Visualizza una rappresentazione di stringa del valore dell'oggetto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ottiene dati aggiuntivi dall'URL (ad esempio, */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Modifica la password per l'utente specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Conferma un account utilizzando il token di conferma dell'account.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crea un nuovo account utente con il nome utente e la password specificati. Per richiedere un token di conferma, passare true per *requireConfirmationToken.To* require a confirmation token, pass true for requireConfirmationToken.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Ottiene l'identificatore intero per l'utente attualmente connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Ottiene il nome dell'utente attualmente connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Genera un token di reimpostazione della password che può essere inviato tramite posta elettronica a un utente in modo che l'utente possa reimpostare la password.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Restituisce l'ID utente dal nome utente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Restituisce true se l'utente corrente è connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Restituisce true se l'utente è stato confermato (ad esempio, tramite un'e-mail di conferma).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Restituisce true se il nome dell'utente corrente corrisponde al nome utente specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Esegue l'accesso all'utente impostando un token di autenticazione nel cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Disconnette l'utente rimuovendo il cookie del token di autenticazione.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Se l'utente non è autenticato, imposta lo stato HTTP su 401 (Non autorizzato).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Se l'utente corrente non è membro di uno dei ruoli specificati, imposta lo stato HTTP su 401 (Non autorizzato).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Se l'utente corrente non è l'utente specificato da *nomeutente*, imposta lo stato HTTP su 401 (Non autorizzato).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Se il token di reimpostazione della password è valido, imposta la password dell'utente con la nuova password.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Data

### `Database.Execute(SQLstatement [,parameters]`

Esegue *SQLstatement* (con parametri facoltativi), ad esempio INSERT, DELETE o UPDATE, e restituisce un conteggio dei record interessati.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Restituisce la colonna di identità dalla riga inserita più di recente.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Apre il file di database specificato o il database specificato utilizzando una stringa di connessione denominata dal file *Web.config.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Apre un database utilizzando la stringa di connessione. In contrasto con `Database.Open`, che utilizza un nome di stringa di connessione.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Esegue una query sul database utilizzando *SQLstatement* (passando facoltativamente parametri) e restituisce i risultati come raccolta.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Esegue *SQLstatement* (con parametri facoltativi) e restituisce un singolo record.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Esegue *SQLstatement* (con parametri facoltativi) e restituisce un singolo valore.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Helper

### `Analytics.GetGoogleHtml(webPropertyId)`

Esegue il rendering del codice JavaScript di Google Analytics per l'ID specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Esegue il rendering del codice JavaScript StatCounter Analytics per il progetto specificato.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Esegue il rendering del codice JavaScript di Yahoo Analytics per l'account specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Passa una ricerca a Bing. Per specificare il sito in cui eseguire la ricerca `Bing.SiteUrl` `Bing.SiteTitle` e un titolo per la casella di ricerca, è possibile impostare le proprietà e . In genere queste proprietà * \_* vengono impostate nella pagina AppStart.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inizializza un grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Aggiunge una legenda a un grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Aggiunge una serie di valori al grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Restituisce un hash per i dati specificati. L'algoritmo `sha256`predefinito è .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Consente agli utenti di Facebook di effettuare una connessione alle pagine.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Esegue il rendering dell'interfaccia utente per il caricamento dei file.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Esegue il rendering del tag del giocatore Xbox specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Esegue il rendering dell'immagine Gravatar per l'indirizzo di posta elettronica specificato.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Converte un oggetto dati in una stringa nel formato JSON (JavaScript Object Notation).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Converte una stringa di input con codifica JSON in un oggetto dati che è possibile scorrere o inserire in un database.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Esegue il rendering dei collegamenti di social networking utilizzando il titolo specificato e l'URL facoltativo.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Associa un messaggio di errore a un campo modulo. Utilizzare `ModelState` l'helper per accedere a questo membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Associa un messaggio di errore a un modulo. Utilizzare `ModelState` l'helper per accedere a questo membro.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Restituisce true se non sono presenti errori di convalida. Utilizzare `ModelState` l'helper per accedere a questo membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Esegue il rendering delle proprietà e dei valori di un oggetto e di qualsiasi oggetto figlio.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Esegue il rendering del test di verifica reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Imposta le chiavi pubbliche e private per il servizio reCAPTCHA. In genere queste proprietà * \_* vengono impostate nella pagina AppStart.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Restituisce il risultato del test reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Esegue il rendering delle informazioni sullo stato di ASP.NET pagine Web.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Esegue il rendering di un flusso Twitter per l'utente specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Esegue il rendering di un flusso Twitter per il testo di ricerca specificato.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Esegue il rendering di un lettore video Flash per il file specificato con larghezza e altezza facoltative.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Esegue il rendering di un lettore Windows Media per il file specificato con larghezza e altezza facoltative.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Esegue il rendering di un lettore Silverlight per il file *xap* specificato con la larghezza e l'altezza richieste.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Restituisce l'oggetto specificato dalla *chiave*o null se l'oggetto non viene trovato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Rimuove l'oggetto specificato dalla *chiave* dalla cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Inserisce il *valore* nella cache sotto il nome specificato dalla *chiave*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crea un `WebGrid` nuovo oggetto utilizzando i dati di una query.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Esegue il rendering del markup per visualizzare i dati in una tabella HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Esegue il rendering `WebGrid` di un pager per l'oggetto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Carica un'immagine dal percorso specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Aggiunge l'immagine specificata come filigrana.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Aggiunge il testo specificato all'immagine.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Capovolge l'immagine orizzontalmente o verticalmente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Carica un'immagine quando un'immagine viene pubblicata su una pagina durante il caricamento di un file.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Ridimensiona un'immagine.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Ruota l'immagine a sinistra o a destra.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Salva l'immagine nel percorso specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Imposta la password per il server SMTP. In genere si imposta * \_* questa proprietà nella pagina AppStart.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Invia un messaggio di posta elettronica.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Imposta il nome del server SMTP. In genere si imposta * \_* questa proprietà nella pagina AppStart.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Imposta il nome utente per il server SMTP. In genere è necessario impostare questa proprietà nella pagina * \_AppStart.Normally* you should set this property in the AppStart page.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Convalida

### `Html.ValidationMessage(field)`

(v2) Per quanto mi liri va in base al Esegue il rendering di un messaggio di errore di convalida per il campo specificato.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Per quanto mi liri va in base al Visualizza un elenco di tutti gli errori di convalida.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Per quanto mi liri va in base al Registra un elemento di input dell'utente per il tipo di convalida specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Per quanto mi liri va in base al Esegue il rendering dinamico degli attributi della classe CSS per la convalida lato client in modo che sia possibile formattare i messaggi di errore di convalida. È necessario fare riferimento alle librerie di script client appropriate e definire le classi CSS.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Per quanto mi liri va in base al Abilita la convalida lato client per il campo di input dell'utente. (Necessario che si faccia riferimento alle librerie di script client appropriate.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Per quanto mi liri va in base al Restituisce true se tutti gli elementi di input dell'utente registrati per la convalida contengono valori validi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Per quanto mi liri va in base al Specifica che gli utenti devono fornire un valore per l'elemento di input dell'utente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Per quanto mi liri va in base al Specifica che gli utenti devono fornire valori per ogni elemento di input dell'utente. Questo metodo non consente di specificare un messaggio di errore personalizzato.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Per quanto mi liri va in base al Specifica un test di convalida `Validation.Add` quando si utilizza il metodo .

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
