---
title: Convalida del modello in ASP.NET Core MVC
author: tdykstra
description: Informazioni sulla convalida del modello in ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 01/14/2019
uid: mvc/models/validation
ms.openlocfilehash: ca7ee54b8e6b6ae5091b0cb133e448ad9c04da8f
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049068"
---
# <a name="model-validation-in-aspnet-core-mvc"></a>Convalida del modello in ASP.NET Core MVC

[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introduzione alla convalida del modello

Prima di essere archiviati in un database, i dati devono essere convalidati dall'app. I dati devono essere controllati per escludere potenziali minacce alla sicurezza. È anche necessario verificare anche che il tipo e le dimensioni dei dati abbiano il formato corretto e che i dati siano conformi alle regole. Nonostante l'implementazione della convalida sia un'attività ridondante e noiosa, la convalida è un processo necessario. In MVC la convalida viene eseguita sia nel client sia nel server.

Fortunatamente, in .NET la convalida viene astratta in attributi di convalida, che contengono codice di convalida, riducendo così la quantità di codice da scrivere.

In ASP.NET Core 2.2 e versioni successive, il runtime di ASP.NET Core blocca (ignora) la convalida se può determinare che un determinato grafo del modello non richiede la convalida. Ignorare la convalida può consentire miglioramenti significativi delle prestazioni durante la convalida di modelli che non possono avere o non hanno controlli server di convalida associati. La convalida ignorata include oggetti quali raccolte di primitive (`byte[]`, `string[]`, `Dictionary<string, string>` e così via) o grafi di oggetti complessi senza controllo server di convalida.

[Visualizzare o scaricare un esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Attributi di convalida

Gli attributi di convalida consentono di configurare la convalida. Concettualmente assomigliano alla convalida nei campi delle tabelle del database. Esistono vincoli, ad esempio l'assegnazione di tipi di dati o campi obbligatori. Altri tipi di convalida includono l'uso di modelli di dati per applicare le regole di business, ad esempio una carta di credito, il numero di telefono o un indirizzo di posta elettronica. Grazie agli attributi di convalida l'applicazione di questi requisiti si rivela più semplice e facile.

Gli attributi di convalida sono specificati a livello di proprietà: 

```csharp
[Required]
public string MyProperty { get; set; }
```

Di seguito è riportato un modello `Movie` con annotazioni relativo a un'app che archivia informazioni su film e programmi televisivi. È necessaria la maggior parte delle proprietà e alcune proprietà stringa devono essere conformi a requisiti di lunghezza. Esiste anche una limitazione per l'intervallo numerico che si applica alla proprietà `Price` da 0 a 999.99 dollari insieme a un attributo di convalida personalizzato.

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

È sufficiente leggere il modello per conoscere le regole relative ai dati per questa app, semplificando così la gestione del codice. Di seguito sono descritti alcuni degli attributi di convalida predefiniti più comuni:

* `[CreditCard]`: convalida che la proprietà abbia un formato di carta di credito.

* `[Compare]`: convalida che due proprietà abbiano corrispondenza in un modello.

* `[EmailAddress]`: convalida che la proprietà abbia un formato di indirizzo di posta elettronica.

* `[Phone]`: convalida che la proprietà abbia un formato di telefono.

* `[Range]`: convalida che il valore della proprietà rientri nell'intervallo specificato.

* `[RegularExpression]`: convalida che i dati corrispondano all'espressione regolare specificata.

* `[Required]`: rende obbligatoria una proprietà.

* `[StringLength]`: convalida che una proprietà stringa non superi la lunghezza massima specificata.

* `[Url]`: convalida che la proprietà abbia un formato di URL.

A scopo di convalida, MVC supporta tutti gli attributi derivati da `ValidationAttribute`. Nello spazio dei nomi [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) è possibile trovare molti attributi di convalida utili.

Esistono delle istanze in cui potrebbero essere necessarie altre funzionalità rispetto a quelle offerte dagli attributi predefiniti. In tali casi, è possibile personalizzare gli attributi di convalida derivando da `ValidationAttribute` o modificando il modello per implementare `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Note sull'uso dell'attributo Required

I [tipi valore](/dotnet/csharp/language-reference/keywords/value-types) non nullable (ad esempio `decimal`, `int`, `float` e `DateTime`) sono intrinsecamente necessari e non richiedono l'attributo `Required`. L'app non esegue controlli di convalida lato server per i tipi non nullable contrassegnati con `Required`.

L'associazione di modelli MVC, che non si occupa di convalida e di attributi di convalida, rifiuta l'invio di campi modulo che contengono un valore mancante o uno spazio vuoto per un tipo non nullable. In assenza di un attributo `BindRequired` nella proprietà di destinazione, l'associazione di modelli ignora i dati mancanti per i tipi non nullable dove il campo modulo manca nei dati modulo in ingresso.

L'[attributo BindRequired](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (vedere anche <xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes>) è utile per verificare che i dati del modulo siano completi. Se è applicato a una proprietà, il sistema di associazione di modelli richiede un valore per tale proprietà. Quando è applicato a un tipo, il sistema di associazione di modelli richiede i valori per tutte le proprietà di quel tipo.

Quando si usa un [tipo Nullable\<T >](/dotnet/csharp/programming-guide/nullable-types/) (ad esempio, `decimal?` o `System.Nullable<decimal>`) e lo si contrassegna con `Required`, viene eseguito un controllo di convalida lato server come se la proprietà fosse un tipo nullable standard, ad esempio `string`.

La convalida lato client richiede un valore per un campo modulo corrispondente a una proprietà del modello che è stata contrassegnata con `Required` e per una proprietà di tipo non nullable che non è stata contrassegnata con `Required`. È possibile usare `Required` per controllare il messaggio di errore di convalida lato client.

::: moniker range=">= aspnetcore-2.1"

## <a name="top-level-node-validation"></a>Convalida del nodo di primo livello

I nodi di primo livello includono:

* Parametri di azione
* Proprietà del controller
* Parametri del gestore di pagina
* Proprietà del modello di pagina

Vengono convalidati i nodi di primo livello associati al modello, oltre a convalidare le proprietà del modello. Nell'esempio seguente dall'app di esempio, il metodo `VerifyPhone` usa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> per convalidare i dati utente nel campo Phone di un modulo:

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_VerifyPhone)]

I nodi di primo livello possono usare <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> con gli attributi di convalida. Nell'esempio seguente dall'app di esempio, il metodo `CheckAge` specifica che il parametro `age` deve essere associato dalla stringa di query quando viene inviato il modulo:

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_CheckAge)]

La pagina Check Age (*CheckAge.cshtml*), include due moduli. Il primo modulo invia un valore `Age` `99` come stringa di query: `https://localhost:5001/Users/CheckAge?Age=99`.

Quando viene inviato un parametro correttamente formattato `age` dalla stringa di query, il modulo viene convalidato.

Il secondo modulo nella pagina Check Age invia il valore `Age` nel corpo della richiesta e la convalida ha esito negativo. L'associazione non riesce perché il parametro `age` deve provenire da una stringa di query.

La convalida è abilitata per impostazione predefinita e controllata dalla proprietà <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> di <xref:Microsoft.AspNetCore.Mvc.MvcOptions>. Per disabilitare la convalida del nodo di primo livello, impostare `AllowValidatingTopLevelNodes` su `false` nelle opzioni di MVC (`Startup.ConfigureServices`):

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

::: moniker-end

## <a name="model-state"></a>Stato del modello

Lo stato del modello rappresenta gli errori di convalida nei valori del modulo HTML inviati.

MVC continua a eseguire la convalida dei campi fino al raggiungimento del numero massimo di errori (200 per impostazione predefinita). È possibile configurare questo numero con il codice seguente in `Startup.ConfigureServices`:

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors)]

## <a name="handle-model-state-errors"></a>Gestire gli errori dello stato del modello

La convalida del modello avviene prima dell'esecuzione di un'azione del controller. È responsabilità dell'azione esaminare `ModelState.IsValid` e rispondere nel modo appropriato. In molti casi, la reazione appropriata consiste nel restituire una risposta di errore, specificando nel dettaglio il motivo per cui è la convalida del modello non è riuscita.

::: moniker range=">= aspnetcore-2.1"

Quando `ModelState.IsValid` restituisce `false` nei controller dell'API Web usando l'attributo `[ApiController]`, viene restituita una risposta HTTP 400 automatica contenente i dettagli del problema. Per altre informazioni, vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses).

::: moniker-end

Alcune app scelgono di seguire una convenzione standard per gestire gli errori di convalida del modello. Nel tal caso il filtro può essere l'elemento adatto in cui implementare tali criteri. È consigliabile testare il comportamento delle azioni in caso di stati del modello validi e non validi.

## <a name="manual-validation"></a>Convalida manuale

Dopo aver completato le operazioni di convalida e associazione di modelli, può essere necessario ripeterne alcune parti. Ad esempio, un utente ha immesso un testo in un campo in cui è previsto un numero intero oppure deve essere calcolato un valore per una proprietà del modello.

Può essere necessario eseguire quindi la convalida manualmente. A tale scopo, chiamare il metodo `TryValidateModel`, come illustrato di seguito:

[!code-csharp[](validation/sample/MoviesController.cs?name=snippet_TryValidateModel)]

## <a name="custom-validation"></a>Convalida personalizzata

Gli attributi di convalida possono essere applicati nella maggior parte dei casi di convalida. Esistono tuttavia regole di business specifiche. È possibile che alcune regole non siano tecniche comuni di convalida dei dati. Può essere ad esempio necessario garantire che un campo sia obbligatorio o che sia conforme a un intervallo di valori. Per questi scenari, gli attributi di convalida personalizzati sono una soluzione efficace. Creare attributi di convalida personalizzati in MVC è un'operazione semplice. È sufficiente ereditare da `ValidationAttribute` ed eseguire l'override del metodo `IsValid`. Il metodo `IsValid` accetta due parametri, il primo è un oggetto denominato *value*, il secondo è un oggetto `ValidationContext` denominato *validationContext*. L'oggetto *value* si riferisce al valore effettivo del campo di cui il validator personalizzato esegue la convalida.

Nell'esempio seguente, sulla base di una regola di business, non è possibile impostare il genere su *Classic* se l'uscita del film è successiva all'anno 1960. L'attributo `[ClassicMovie]` prima controlla il genere e, nel caso in cui sia un classico, verifica se la data di uscita è successiva al 1960. Se il film è uscito dopo il 1960, la convalida non riesce. L'attributo accetta un parametro integer rappresentante l'anno che è possibile usare per convalidare i dati. Il valore del parametro può essere acquisito nel costruttore dell'attributo, come illustrato di seguito:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

La variabile `movie` specificata sopra rappresenta un oggetto `Movie` contenente i dati dell'invio del modulo da convalidare. In questo caso il codice di convalida controlla la data e il genere nel metodo `IsValid` della classe `ClassicMovieAttribute` sulla base delle regole. Se la convalida ha esito positivo,`IsValid` restituisce un codice `ValidationResult.Success`. Se la convalida non ha esito positivo, viene restituito un codice `ValidationResult` con un messaggio di errore:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?name=snippet_GetErrorMessage)]

Quando un utente modifica il campo `Genre` e invia il modulo, il metodo `IsValid` della classe `ClassicMovieAttribute` verifica se il film sia un classico. Come fosse un attributo predefinito, applicare la classe `ClassicMovieAttribute` a una proprietà, ad esempio `ReleaseDate`, per garantire che la convalida venga eseguita, come illustrato nell'esempio di codice precedente. Poiché l'esempio funziona solo con tipi `Movie`, è consigliabile usare `IValidatableObject` come illustrato nel paragrafo seguente.

In alternativa, questo stesso codice può essere inserito nel modello implementando il metodo `Validate` nell'interfaccia `IValidatableObject`. Mentre gli attributi di convalida personalizzati sono adatti per la convalida di singole proprietà, l'implementazione di `IValidatableObject` può essere usata per implementare la convalida a livello di classe:

[!code-csharp[](validation/sample/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="client-side-validation"></a>Convalida lato client

La convalida lato client è un'operazione molto comoda. Consente infatti agli utenti di ridurre i tempi di attesa di un round trip a un server. In termini di business, anche poche frazioni di secondo moltiplicate per centinaia di volte al giorno contribuiscono all'aumento di tempo, costi e frustrazione. Grazie a un processo di convalida semplice e immediato, gli utenti possono lavorare in modo più efficiente e garantire input e output di qualità elevata.

Perché la convalida lato client sia eseguita come illustrato di seguito, è necessario avere una visualizzazione con i riferimenti agli script JavaScript corretti.

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

Lo script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) è una libreria front-end Microsoft personalizzata che si basa sul noto plug-in [jQuery Validate](https://jqueryvalidation.org/). Senza Query Unobtrusive Validation, sarebbe necessario scrivere il codice della stessa logica di convalida in due posizioni, vale a dire negli attributi di convalida lato server nelle proprietà del modello e nuovamente negli script lato client. Gli esempi del metodo [`validate()`](https://jqueryvalidation.org/validate/) jQuery Validate illustrano quanto sarebbe complessa questa operazione. Invece, grazie agli [helper tag](xref:mvc/views/tag-helpers/intro)e agli [helper HTML](xref:mvc/views/overview) di MVC è possibile usare gli attributi di convalida e i metadati di tipo delle proprietà del modello per eseguire il rendering degli [attributi data-](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) HTML 5 negli elementi del modulo che devono essere convalidati. MVC genera gli attributi `data-` sia per gli attributi predefiniti sia per quelli personalizzati. jQuery Unobtrusive Validation analizza quindi gli attributi `data-` e passa la logica a jQuery Validate, "copiando" in modo efficace la logica di convalida lato server nel client. È possibile visualizzare gli errori di convalida nel client tramite gli helper tag rilevanti, come illustrato di seguito:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

Gli helper tag precedenti eseguono il rendering del modulo HTML specificato di seguito. Si noti che gli attributi `data-` nell'output HTML corrispondono agli attributi di convalida per la proprietà `ReleaseDate`. L'attributo `data-val-required` seguente contiene un messaggio di errore che viene visualizzato se l'utente non compila il campo relativo alla data di uscita. Lo script jQuery Unobtrusive Validation passa questo valore al metodo [`required()`](https://jqueryvalidation.org/required-method/) jQuery Validate, che visualizza il messaggio nell'elemento **\<span>** di accompagnamento.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

La convalida lato client non consente l'invio del modulo finché non è ritenuto valido. Tramite il pulsante Invia viene eseguito JavaScript, che invia il modulo oppure visualizza i messaggi di errore.

MVC determina i valori attributo del tipo sulla base del tipo di dati .NET di una proprietà, di cui è stato possibilmente eseguito l'override tramite gli attributi `[DataType]`. L'attributo `[DataType]` di base non esegue la vera convalida lato server. I browser selezionano i messaggi di errore e visualizzano gli errori a proprio piacimento. Il pacchetto jQuery Validation Unobtrusive può tuttavia eseguire l'override dei messaggi e visualizzarli insieme ad altri. Questo avviene soprattutto quando gli utenti applicano le sottoclassi `[DataType]`, ad esempio `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Aggiungere la convalida a moduli dinamici

Quando la pagina viene caricata, jQuery Unobtrusive Validation passa la logica e i parametri di convalida a jQuery Validate. I moduli generati in modo dinamico non presentano quindi automaticamente la convalida. È invece necessario chiedere a jQuery Unobtrusive Validation di analizzare il modulo dinamico immediatamente dopo essere stato creato. Il codice riportato di seguito illustra ad esempio come configurare la convalida lato client in un modulo che è stato aggiunto tramite AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

Il metodo `$.validator.unobtrusive.parse()` accetta un selettore jQuery per ogni argomento. Questo metodo indica a jQuery Unobtrusive Validation di analizzare gli attributi `data-` dei moduli all'interno di tale tipo di selettore. I valori di questi attributi vengono quindi passati al plug-in jQuery Validate in modo che il modulo presenti le regole di convalida lato client necessarie.

### <a name="add-validation-to-dynamic-controls"></a>Aggiungere la convalida a controlli dinamici

È anche possibile aggiornare le regole di convalida in un modulo quando singoli controlli, ad esempio `<input/>` e `<select/>`, vengono generati in modo dinamico. Non è possibile passare direttamente i selettori per questi elementi al metodo `parse()` perché il modulo adiacente è già stato analizzato e non sarà aggiornato. È quindi necessario rimuovere i dati di convalida esistenti, quindi rianalizzare l'intero modulo, come illustrato di seguito:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

È possibile creare una logica lato client per l'attributo personalizzato, in modo che la [convalida discreta](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) (che crea un adattatore per la [convalida jQuery](http://jqueryvalidation.org/documentation/)) la esegua automaticamente nel client come parte del processo di convalida. Prima di tutto è necessario controllare quali attributi data- vengono aggiunti implementando l'interfaccia `IClientModelValidator`, come illustrato di seguito:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?name=snippet_AddValidation)]

Gli attributi che implementano questa interfaccia possono aggiungere attributi HTML ai campi generati. Esaminando l'output dell'elemento `ReleaseDate` si può notare che il codice HTML è simile all'esempio precedente, ad eccezione di un attributo `data-val-classicmovie` che è stato definito nel metodo `AddValidation` di `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

La convalida discreta usa i dati negli attributi `data-` per visualizzare i messaggi di errore. jQuery non riconosce tuttavia le regole o i messaggi finché non vengono aggiunti all'oggetto `validator` di jQuery, Questa operazione è illustrata nell'esempio seguente, che aggiunge una metodo di convalida client `classicmovie` personalizzato all'oggetto jQuery `validator`. Per una spiegazione del metodo `unobtrusive.adapters.add`, vedere [Unobtrusive Client Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) (Unobtrusive Client Validation in MVC ASP.NET).

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?name=snippet_UnobtrusiveValidation)]

Con il codice precedente, il metodo `classicmovie` esegue la convalida lato client alla data di pubblicazione del film. Viene visualizzato il messaggio di errore se il metodo restituisce `false`.

## <a name="remote-validation"></a>Convalida remota

La convalida remota è una funzionalità utile da usare quando è necessario convalidare i dati nel client rispetto ai dati nel server. Ad esempio, l'app deve verificare se un nome utente o un indirizzo di posta elettronica è già in uso. A tale scopo deve eseguire una query su un enorme quantitativo di dati. Per scaricare grandi set di dati per la convalida di uno o di alcuni campi sono necessarie troppe risorse e si corre il rischio di esporre informazioni riservate. In alternativa è possibile richiedere un round trip per la convalida di un campo.

La convalida remota può essere implementata in un processo a due fasi. Prima di tutto è necessario annotare il modello con l'attributo `[Remote]`. L'attributo `[Remote]` accetta più overload da usare per indirizzare JavaScript lato client al codice appropriato da chiamare. Nell'esempio seguente si fa riferimento al metodo di azione `VerifyEmail` del controller `Users`.

[!code-csharp[](validation/sample/User.cs?name=snippet_UserEmailProperty)]

Il secondo passaggio prevede l'inserimento di codice di convalida nel metodo di azione corrispondente come definito nell'attributo `[Remote]`. In base alla documentazione del metodo [remote](https://jqueryvalidation.org/remote-method/) di jQuery Validate, la risposta del server deve essere una stringa JSON che può essere:

* `"true"` per gli elementi validi.
* `"false"`, `undefined` o `null` per gli elementi non validi, usando il messaggio di errore predefinito.

Se la risposta del server è una stringa (ad esempio, `"That name is already taken, try peter123 instead"`), la stringa viene visualizzata come messaggio di errore personalizzato al posto della stringa predefinita.

La definizione del metodo `VerifyEmail` segue le regole illustrate di seguito. Restituisce un messaggio di errore di convalida se l'indirizzo di posta elettronica è usato oppure `true` se l'indirizzo è libero, ed esegue il wrapping del risultato in un oggetto `JsonResult`. Il lato client può usare il valore restituito per continuare o visualizzare l'errore se necessario.

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_VerifyEmail)]

A questo punto, quando viene immesso un indirizzo di posta elettronica, JavaScript esegue nella visualizzazione una chiamata remota per verificare se l'indirizzo è in uso, e in caso affermativo, visualizza il messaggio di errore. In caso contrario, è possibile inviare il modulo come di consueto.

La proprietà `[Remote]` dell'attributo `AdditionalFields` è utile per la convalida di combinazioni di campi rispetto ai dati nel server. Ad esempio, se il modello `User` precedente avesse due proprietà aggiuntive denominate `FirstName` e `LastName`, potrebbe essere necessario controllare che non siano esistenti utenti con la stessa coppia di nomi. Definire le nuove proprietà come illustrato nel codice seguente:

[!code-csharp[](validation/sample/User.cs?name=snippet_UserNameProperties)]

Non è stato possibile impostare in modo esplicito `AdditionalFields` sulle stringhe `"FirstName"` e `"LastName"`, ma tramite l'operatore [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) il successivo refactoring risulta semplificato. Il metodo di azione per eseguire la convalida deve accettare due argomenti, uno per il valore di `FirstName` e uno per il valore di `LastName`.

[!code-csharp[](validation/sample/UsersController.cs?name=snippet_VerifyName)]

A questo punto quando viene immesso un nome e un cognome, JavaScript:

* Effettua una chiamata remota per controllare se la coppia di nomi è stata usata.
* Se la coppia è stata usata, viene visualizzato un messaggio di errore.
* In caso contrario, l'utente può inviare il modulo.

Se si vuole convalidare due o più campi aggiuntivi con l'attributo `[Remote]`,specificarli sotto forma di elenco delimitato da virgole. Ad esempio, per aggiungere la proprietà `MiddleName` al modello, impostare l'attributo `[Remote]` come illustrato nel codice seguente:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, come tutti gli argomenti dell'attributo, deve essere un'espressione costante. Non è quindi necessario usare una [stringa interpolata](/dotnet/csharp/language-reference/keywords/interpolated-strings) oppure chiamare <xref:System.String.Join*> per inizializzare `AdditionalFields`. Per ogni altro campo aggiunto all'attributo `[Remote]`, è necessario aggiungere un altro argomento al metodo di azione del controller corrispondente.
