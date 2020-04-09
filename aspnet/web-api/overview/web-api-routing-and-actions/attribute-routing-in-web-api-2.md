---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing degli attributi nellASP.NETAPI Web 2 Documenti Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: bb68fe8e6769619029a3fa039d6f0d6f3303afbe
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675391"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Routing degli attributi nellASP.NETAPI Web 2

da parte di [Mike Wasson](https://github.com/MikeWasson)

*Il routing* è il modo in cui l'API Web associa un URI a un'azione. Web API 2 supporta un nuovo tipo di routing, denominato *routing degli attributi*. Come implica il nome, il routing degli attributi utilizza gli attributi per definire le route. Il routing degli attributi offre un maggiore controllo sugli URI nell'API Web. Ad esempio, è possibile creare facilmente URI che descrivono gerarchie di risorse.

Lo stile precedente di routing, denominato routing basato su convenzioni, è ancora completamente supportato. Infatti, è possibile combinare entrambe le tecniche nello stesso progetto.

In questo argomento viene illustrato come abilitare il routing degli attributi e vengono descritte le varie opzioni per il routing degli attributi. Per un'esercitazione end-to-end che usa il routing degli attributi, vedere [Creare un'API REST con routing degli attributi in Web API 2.](create-a-rest-api-with-attribute-routing.md)

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Edizione Community, Professional o Enterprise

In alternativa, usare NuGet Package Manager per installare i pacchetti necessari. Dal menu **Strumenti** di Visual Studio, selezionare **NuGet Package Manager**, quindi selezionare Console gestione **pacchetti**. Immettere il comando seguente nella finestra della console di Gestione pacchetti:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Perché il routing degli attributi?

La prima versione di Web API utilizzava il routing *basato su convenzioni.* In questo tipo di routing, si definiscono uno o più modelli di route, che sono fondamentalmente stringhe con parametri. Quando il framework riceve una richiesta, corrisponde all'URI rispetto al modello di route. Per ulteriori informazioni sul routing basato su convenzioni, vedere [Routing in ASP.NETAPI Web](routing-in-aspnet-web-api.md).

Uno dei vantaggi del routing basato su convenzioni è che i modelli vengono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente tra tutti i controller. Sfortunatamente, il routing basato su convenzioni rende difficile il supporto di determinati modelli URI comuni nelle API RESTful. Ad esempio, le risorse spesso contengono risorse figlio: i clienti hanno ordini, i film hanno attori, i libri hanno autori e così via. È naturale creare URI che riflettono queste relazioni:It's natural to create URIs that reflect these relations:

`/customers/1/orders`

Questo tipo di URI è difficile da creare utilizzando il routing basato su convenzioni. Anche se può essere fatto, i risultati non scalabili bene se si dispone di molti controller o tipi di risorse.

Con il routing degli attributi, è semplice definire una route per questo URI. È sufficiente aggiungere un attributo all'azione del controller:You simply add an attribute to the controller action:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Ecco alcuni altri modelli che il routing degli attributi semplifica.

**Controllo delle versioni API**

In questo esempio, "/api/v1/products" verrebbe instradato a un controller diverso da "/api/v2/products".

`/api/v1/products`
`/api/v2/products`

**Segmenti URI di overload**

In questo esempio, "1" è un numero di ordine, ma "in sospeso" esegue il mapping a una raccolta.

`/orders/1`
`/orders/pending`

**Più tipi di parametri**

In questo esempio, "1" è un numero d'ordine, ma "2013/06/16" specifica una data.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Abilitazione del routing degli attributi

Per abilitare il routing degli attributi, chiamare **MapHttpAttributeRoutes** durante la configurazione. Questo metodo di estensione è definito nella classe **System.Web.Http.HttpConfigurationExtensions.**

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Il routing degli attributi può essere combinato con il routing [basato su convenzioni.](routing-in-aspnet-web-api.md) Per definire route basate su convenzioni, chiamare il **MapHttpRoute** metodo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Per ulteriori informazioni sulla configurazione dell'API Web, vedere [Configurazione di ASP.NET API Web 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: migrazione da API Web 1

Prima di Web API 2, i modelli di progetto API Web generano codice simile al seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Se il routing degli attributi è abilitato, questo codice genererà un'eccezione. Se si aggiorna un progetto API Web esistente per utilizzare il routing degli attributi, assicurarsi di aggiornare questo codice di configurazione al seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Per ulteriori informazioni, vedere [Configurazione dell'API Web con ASP.NET hosting](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Aggiunta di attributi di percorso

Di seguito è riportato un esempio di route definita utilizzando un attributo:Here is an example of a route defined using an attribute:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La &quot;stringa customers/'customerId'/orders&quot; è il modello URI per la route. Api Web tenta di associare l'URI della richiesta al modello. In questo esempio, "customers" e "orders" sono segmenti letterali e "'customerId'" è un parametro di variabile. I seguenti URI corrisponderebbero a questo modello:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

È possibile limitare la corrispondenza utilizzando [i vincoli](#constraints)descritti più avanti in questo argomento.

Si noti che &quot;&quot; il parametro "customerId" nel modello di route corrisponde al nome del parametro *customerId* nel metodo. Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route. Ad esempio, se `http://example.com/customers/1/orders`l'URI è , l'API Web tenta di associare il valore "1" al parametro *customerId* nell'azione.

Un modello URI può avere diversi parametri:A URI template can have several parameters:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Tutti i metodi del controller che non dispongono di un attributo di route utilizzano il routing basato su convenzione. In questo modo, è possibile combinare entrambi i tipi di ciclo nello stesso progetto.

## <a name="http-methods"></a>Metodi HTTP

API Web seleziona anche le azioni in base al metodo HTTP della richiesta (GET, POST e così via). Per impostazione predefinita, l'API Web cerca una corrispondenza senza distinzione tra maiuscole e minuscole con l'inizio del nome del metodo del controller. Ad esempio, un `PutCustomers` metodo del controller denominato corrisponde a una richiesta HTTP PUT.

È possibile eseguire l'override di questa convenzione decorando il metodo con i seguenti attributi:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Per tutti gli altri metodi HTTP, inclusi i metodi non standard, utilizzare l'attributo **AcceptVerbs,** che accetta un elenco di metodi HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefissi di route

Spesso, le route in un controller iniziano tutte con lo stesso prefisso. Ad esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

È possibile impostare un prefisso comune per un intero controller usando l'attributo **[RoutePrefix]:You** can set a common prefix for an entire controller by using the [RoutePrefix] attribute:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Utilizzare una tilde (-) sull'attributo del metodo per eseguire l'override del prefisso della route:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Il prefisso della route può includere parametri:The route prefix can include parameters:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Vincoli di route

I vincoli di route consentono di limitare la corrispondenza dei parametri nel modello di percorso. La sintassi &quot;generale è&quot;"parameter:constraint" . Ad esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

In questo caso, la prima &quot;route&quot; verrà selezionata solo se il segmento id dell'URI è un numero intero. In caso contrario, verrà scelto il secondo percorso.

Nella tabella seguente sono elencati i vincoli supportati.

| Vincolo | Descrizione | Esempio |
| --- | --- | --- |
| alpha | Corrisponde ai caratteri dell'alfabeto latino maiuscolo o minuscolo (a-z, A-z) | x:alfa |
| bool | Corrisponde a un valore booleano. | X:bool |
| Datetime | Corrisponde a un valore **DateTime.** | x:datetime |
| decimal | Corrisponde a un valore decimale. | x:decimale |
| double | Corrisponde a un valore a virgola mobile a 64 bit. | x:double |
| float | Corrisponde a un valore a virgola mobile a 32 bit. | x:float |
| guid | Corrisponde a un valore GUID. | x:guid |
| INT | Corrisponde a un valore integer a 32 bit. | X:int |
| length | Corrisponde a una stringa con la lunghezza specificata o all'interno di un intervallo di lunghezze specificato. | x:lunghezza(6) x:length(1,20) |
| long | Corrisponde a un valore integer a 64 bit. | x:long |
| max | Corrisponde a un numero intero con un valore massimo. | x:max(10) |
| Maxlength | Corrisponde a una stringa con una lunghezza massima. | x:maxlength(10) |
| Min | Corrisponde a un numero intero con un valore minimo. | x:min(10) |
| Minlength | Corrisponde a una stringa con una lunghezza minima. | x:minlength(10) |
| range | Corrisponde a un numero intero compreso in un intervallo di valori. | x:intervallo(10,50) |
| regex | Corrisponde a un'espressione regolare. | {3}x:regex('d'd'd'd'){4}{3} |

Si noti che alcuni &quot;vincoli, ad esempio min&quot;, accettano argomenti tra parentesi. È possibile applicare più vincoli a un parametro, separati da due punti.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Vincoli di route personalizzati

È possibile creare vincoli di route personalizzati implementando l'interfaccia **IHttpRouteConstraint.You** can create custom route constraints by implementing the IHttpRouteConstraint interface. Ad esempio, il vincolo seguente limita un parametro a un valore intero diverso da zero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Nel codice seguente viene illustrato come registrare il vincolo:The following code shows how to register the constraint:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Ora è possibile applicare il vincolo nei percorsi:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

È inoltre possibile sostituire l'intera classe **DefaultInlineConstraintResolver** implementando l'interfaccia **IInlineConstraintResolver.** In questo modo verranno sostituiti tutti i vincoli incorporati, a meno che l'implementazione di **IInlineConstraintResolver** li aggiunge in modo specifico.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parametri URI facoltativi e valori predefinitiOptional URI Parameters and Default Values

È possibile rendere facoltativo un parametro URI aggiungendo un punto interrogativo al parametro route. Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In questo `/api/books/locale/1033` esempio `/api/books/locale` e restituire la stessa risorsa.

In alternativa, è possibile specificare un valore di default all'interno del modello di ciclo di lavorazione, come indicato di seguito:Alternatively, you can specify a default value inside the route template, as follows:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Questo è quasi lo stesso dell'esempio precedente, ma c'è una leggera differenza di comportamento quando viene applicato il valore predefinito.

- Nel primo esempio di codice ( , lcid:int?, ), il valore predefinito di 1033 viene assegnato direttamente al parametro del metodo , pertanto il parametro avrà questo valore esatto.
- Nel secondo esempio ( , lcid:int , 1033 "), il valore predefinito di "1033" passa attraverso il processo di associazione del modello. Il model-binder predefinito convertirà "1033" nel valore numerico 1033. Tuttavia, è possibile collegare un raccoglitore di modelli personalizzato, che potrebbe eseguire un'operazione diversa.

Nella maggior parte dei casi, a meno che non si disponga di raccoglitori di modelli personalizzati nella pipeline, i due formati saranno equivalenti.

<a id="route-names"></a>
## <a name="route-names"></a>Nomi delle route

Nell'API Web, ogni route ha un nome. I nomi delle route sono utili per generare collegamenti, in modo che sia possibile includere un collegamento in una risposta HTTP.

Per specificare il nome della route, impostare la proprietà **Name** sull'attributo. Nell'esempio seguente viene illustrato come impostare il nome della route e come utilizzare il nome della route durante la generazione di un collegamento.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordine ciclo di lavorazione

Quando il framework tenta di trovare una corrispondenza con un URI con una route, valuta le route in un ordine specifico. Per specificare l'ordine, impostare la proprietà **Order** sull'attributo del ciclo di lavorazione. I valori inferiori vengono valutati per primi. Il valore di ordine predefinito è zero.

Ecco come viene determinato l'ordine totale:

1. Confrontare la proprietà **Order** dell'attributo della route.
2. Esaminare ogni segmento URI nel modello di route. Per ogni segmento, ordinare come segue:

    1. Segmenti letterali.
    2. Parametri del ciclo di lavorazione con vincoli.
    3. Parametri di percorso senza vincoli.
    4. Segmenti di parametro con caratteri jolly con vincoli.
    5. Segmenti di parametro con caratteri jolly senza vincoli.
3. Nel caso di un collegamento, le route vengono ordinate in base a un confronto di stringhe ordinale senza distinzione tra maiuscole e minuscole ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.

Ecco un esempio. Si supponga di definire il controller seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Questi percorsi sono ordinati come segue.

1. ordini/dettagli
2. ordini/ id
3. ordini/ nomecliente
4. ordini/data\*di
5. ordini/in sospeso

Si noti che "dettagli" è un segmento letterale e viene visualizzato prima di "id" ma "in sospeso" viene visualizzato per ultimo perché il **Order** proprietà è 1. In questo esempio si presuppone che non siano presenti clienti denominati "dettagli" o "in sospeso". In generale, cercare di evitare percorsi ambigui. In questo esempio, un `GetByCustomer` modello di route migliore per è "customers/'customerName'" )
