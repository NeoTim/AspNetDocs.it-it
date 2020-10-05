---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Supporto delle opzioni di query OData in API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Panoramica con esempi di codice mostra le opzioni di query OData di supporto in API Web ASP.NET 2 per ASP.NET 4. x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 96820fab7ac89885058962f44ded86cb0184ee97
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2020
ms.locfileid: "86188625"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Supporto delle opzioni di query OData in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

Questa panoramica con esempi di codice illustra le opzioni di query OData di supporto in API Web ASP.NET 2 per ASP.NET 4. x. 

OData definisce i parametri che possono essere usati per modificare una query OData. Il client invia questi parametri nella stringa di query dell'URI della richiesta. Per ordinare i risultati, ad esempio, un client usa il parametro $orderby:

`http://localhost/Products?$orderby=Name`

La specifica OData chiama queste *Opzioni di query*dei parametri. È possibile abilitare le opzioni di query OData per qualsiasi controller API Web nel progetto &#8212; non è necessario che il controller sia un endpoint OData. Si tratta di un modo pratico per aggiungere funzionalità come il filtro e l'ordinamento a qualsiasi applicazione API Web.

Prima di abilitare le opzioni di query, vedere l'argomento [Guida alla sicurezza di OData](odata-security-guidance.md).

- [Abilitazione di opzioni di query OData](#enable)
- [Query di esempio](#examples)
- [Paging basato su server](#server-paging)
- [Limitazione delle opzioni di query](#limiting_query_options)
- [Richiamo diretto delle opzioni query](#ODataQueryOptions)
- [Convalida query](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Abilitazione di opzioni di query OData

API Web supporta le seguenti opzioni di query OData:

| Opzione | Descrizione |
| --- | --- |
| $expand | Espande le entità correlate inline. |
| $filter | Filtra i risultati in base a una condizione booleana. |
| $inlinecount | Indica al server di includere il conteggio totale delle entità corrispondenti nella risposta. (Utile per il paging lato server). |
| $orderby | Ordina i risultati. |
| $select | Consente di selezionare le proprietà da includere nella risposta. |
| $skip | Ignora i primi n risultati. |
| $top | Restituisce solo i primi n risultati. |

Per usare le opzioni di query OData, è necessario abilitarle in modo esplicito. È possibile abilitarli globalmente per l'intera applicazione o abilitarli per controller specifici o azioni specifiche.

Per abilitare le opzioni di query OData globalmente, chiamare **EnableQuerySupport** sulla classe **HttpConfiguration** all'avvio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

Il metodo **EnableQuerySupport** Abilita le opzioni di query a livello globale per qualsiasi azione del controller che restituisce un tipo **IQueryable** . Se non si desidera che le opzioni di query siano abilitate per l'intera applicazione, è possibile abilitarle per azioni specifiche del controller aggiungendo l'attributo **[Queryable]** al metodo di azione.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Query di esempio

Questa sezione illustra i tipi di query possibili usando le opzioni di query OData. Per dettagli specifici sulle opzioni di query, vedere la documentazione di OData in [www.OData.org](http://www.odata.org/).

Per informazioni su $expand e $select, vedere [utilizzo di $Select, $Expand e $value in API Web ASP.NET OData](using-select-expand-and-value.md).

**Paging basato su client**

Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati. Ad esempio, un client potrebbe visualizzare 10 voci alla volta, con collegamenti "Avanti" per ottenere la pagina di risultati successiva. A tale scopo, il client utilizza le opzioni $top e $skip.

`http://localhost/Products?$top=10&$skip=20`

L'opzione $top indica il numero massimo di voci da restituire e l'opzione $skip indica il numero di voci da ignorare. Nell'esempio precedente vengono recuperate le voci da 21 a 30.

**Filtro**

L'opzione $filter consente a un client di filtrare i risultati applicando un'espressione booleana. Le espressioni di filtro sono piuttosto potenti. sono inclusi operatori logici e aritmetici, funzioni stringa e funzioni di data.

| Restituisce tutti i prodotti con categoria uguale a "Toys". | `http://localhost/Products?$filter=Category` EQ ' Toys ' |
| --- | --- |
| Restituisce tutti i prodotti con prezzo inferiore a 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Operatori logici: restituisce tutti i prodotti in cui Price >= 5 e Price <= 15. | `http://localhost/Products?$filter=Price` GE 5 e Price le 15 |
| Funzioni stringa: restituisce tutti i prodotti con "ZZ" nel nome. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funzioni di data: restituiscono tutti i prodotti con rilasciato dopo 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Ordinamento**

Per ordinare i risultati, usare il filtro $orderby.

| Ordina per prezzo. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Ordina per prezzo in ordine decrescente (dal più alto al più basso). | `http://localhost/Products?$orderby=Price desc` |
| Ordina per categoria, quindi Ordina per prezzo in ordine decrescente all'interno delle categorie. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paging basato su server

Se il database contiene milioni di record, non è opportuno inviarli tutti in un unico payload. Per evitare questo problema, il server può limitare il numero di voci che invia in una singola risposta. Per abilitare il paging del server, impostare la proprietà **pageSize** nell'attributo **Queryable** . Il valore è il numero massimo di voci da restituire.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Se il controller restituisce il formato OData, il corpo della risposta conterrà un collegamento alla pagina di dati successiva:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Il client può usare questo collegamento per recuperare la pagina successiva. Per conoscere il numero totale di voci nel set di risultati, il client può impostare l'opzione query $inlinecount con il valore "AllPages".

`http://localhost/Products?$inlinecount=allpages`

Il valore "AllPages" indica al server di includere il conteggio totale nella risposta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> I collegamenti a pagina successiva e il conteggio inline richiedono entrambi il formato OData. Il motivo è che OData definisce campi speciali nel corpo della risposta per conservare il collegamento e il conteggio.

Per i formati non OData, è ancora possibile supportare i collegamenti della pagina successiva e il conteggio inline, eseguendo il wrapping dei risultati della query in un oggetto **PageResult &lt; T &gt; ** . Tuttavia, richiede un po' di codice. Esempio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Di seguito è riportato un esempio di risposta JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitazione delle opzioni di query

Le opzioni di query offrono al client un elevato controllo sulla query eseguita sul server. In alcuni casi, potrebbe essere necessario limitare le opzioni disponibili per motivi di sicurezza o di prestazioni. L'attributo **[Queryable]** include alcune proprietà predefinite per questo oggetto. Di seguito sono riportati alcuni esempi.

Consenti solo $skip e $top, per supportare il paging e nient'altro:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Consentire l'ordinamento solo in base a determinate proprietà, per impedire l'ordinamento delle proprietà non indicizzate nel database:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Consenti la funzione logica "EQ", ma non altre funzioni logiche:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Non consentire operatori aritmetici:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

È possibile limitare le opzioni a livello globale costruendo un'istanza di **QueryableAttribute** e passandola alla funzione **EnableQuerySupport** :

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Richiamo diretto delle opzioni query

Anziché usare l'attributo **[Queryable]** , è possibile richiamare le opzioni di query direttamente nel controller. A tale scopo, aggiungere un parametro **ODataQueryOptions** al metodo controller. In questo caso, non è necessario l'attributo **[Queryable]** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

L'API Web popola **ODataQueryOptions** dalla stringa di query URI. Per applicare la query, passare un oggetto **IQueryable** al metodo **ApplyTo** . Il metodo restituisce un altro oggetto **IQueryable**.

Per gli scenari avanzati, se non si dispone di un provider di query **IQueryable** , è possibile esaminare il **ODataQueryOptions** e tradurre le opzioni di query in un altro formato. Ad esempio, vedere il post di Blog di Nigro per la [conversione di query OData in HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), che include anche un [esempio](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).

<a id="query-validation"></a>
## <a name="query-validation"></a>Convalida query

L'attributo **[Queryable]** convalida la query prima di eseguirla. Il passaggio di convalida viene eseguito nel metodo **QueryableAttribute. ValidateQuery** . È anche possibile personalizzare il processo di convalida.

Vedere anche [informazioni aggiuntive sulla sicurezza di OData](odata-security-guidance.md).

In primo luogo, eseguire l'override di una delle classi Validator definite nello spazio dei nomi **Web. http. OData. query. Validators** . Ad esempio, la classe Validator seguente disabilita l'opzione "DESC" per l'opzione $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Sottoclassare l'attributo **[Queryable]** per eseguire l'override del metodo **ValidateQuery** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Impostare quindi l'attributo personalizzato globalmente o per controller:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Se si usa direttamente **ODataQueryOptions** , impostare il validator sulle opzioni:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
