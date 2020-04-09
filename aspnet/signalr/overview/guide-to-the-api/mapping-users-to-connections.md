---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mappatura degli utenti SignalR alle connessioni Documenti Microsoft
author: bradygaster
description: In questo argomento viene illustrato come conservare le informazioni sugli utenti e le relative connessioni. Patrick Fletcher ha contribuito a scrivere questo argomento. Versioni software utilizzate in questo argomento...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676160"
---
# <a name="mapping-signalr-users-to-connections"></a>Mapping degli utenti di SignalR alle connessioni

 di [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene illustrato come conservare le informazioni sugli utenti e le relative connessioni.
>
> Patrick Fletcher ha contribuito a scrivere questo argomento.
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

## <a name="introduction"></a>Introduzione

Ogni client che si connette a un hub passa un ID di connessione univoco. È possibile recuperare questo `Context.ConnectionId` valore nella proprietà del contesto dell'hub. Se l'applicazione deve eseguire il mapping di un utente all'ID di connessione e rendere persistente il mapping, è possibile utilizzare uno degli elementi seguenti:

- [Il provider di ID utente (SignalR 2)](#IUserIdProvider)
- [Archiviazione in memoria](#inmemory), ad esempio un dizionario
- [Gruppo SignalR per ogni utente](#groups)
- [Archiviazione esterna permanente,](#database)ad esempio una tabella di database o un'archiviazione tabelle di AzurePermanent, external storage , such as a database table or Azure table storage

Ognuna di queste implementazioni viene illustrata in questo argomento. Utilizzare i `OnConnected` `OnDisconnected`metodi `OnReconnected` , e `Hub` della classe per tenere traccia dello stato della connessione utente.

L'approccio migliore per l'applicazione dipende da:

- Il numero di server Web che ospitano l'applicazione.
- Se è necessario ottenere un elenco degli utenti attualmente connessi.
- Se è necessario rendere persistenti le informazioni di gruppo e utente al riavvio dell'applicazione o del server.
- Se la latenza della chiamata a un server esterno è un problema.

Nella tabella seguente viene illustrato quale approccio funziona per queste considerazioni.

|  | Più di un server | Ottenere l'elenco degli utenti attualmente connessiGet list of currently connected users | Rendere persistenti le informazioni dopo il riavvioPersist information after restarts | Prestazioni ottimali |
| --- | --- | --- | --- | --- |
| UserID Provider | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| In memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Gruppi di utenti singoli | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, esterna | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Provider IUserID

Questa funzionalità consente agli utenti di specificare ciò che l'id utente è basato su un IRequest tramite una nuova interfaccia IUserIdProvider.This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.

**The IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Per impostazione predefinita, sarà presente un'implementazione che utilizza l'utente `IPrincipal.Identity.Name` come nome utente. Per modificare questa impostazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Dall'interno di un hub, sarai in grado di inviare messaggi a questi utenti tramite la seguente API:

**Invio di un messaggio a un utente specifico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Archiviazione in memoria

Negli esempi seguenti viene illustrato come mantenere le informazioni di connessione e utente in un dizionario archiviato in memoria. Il dizionario `HashSet` utilizza un per archiviare l'ID di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione SignalR. Ad esempio, un utente connesso tramite più dispositivi o più schede del browser avrebbe più di un ID di connessione.

Se l'applicazione viene arrestata, tutte le informazioni andranno perse, ma verranno ripopolate man mano che gli utenti ristabiliscono le connessioni. L'archiviazione in memoria non funziona se l'ambiente include più di un server Web perché ogni server avrebbe una raccolta separata di connessioni.

Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni. La chiave per il HashSet sarà il nome dell'utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Nell'esempio successivo viene illustrato come utilizzare la classe di mapping delle connessioni da un hub. L'istanza della classe viene archiviata in un nome `_connections`di variabile .

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Gruppi di utenti singoli

È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente. Il nome di ogni gruppo è il nome dell'utente. Se un utente dispone di più di una connessione, ogni ID di connessione viene aggiunto al gruppo dell'utente.

Non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette. Questa azione viene eseguita automaticamente dal framework SignalR.

Nell'esempio seguente viene illustrato come implementare gruppi di utenti singoli.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Stoccaggio permanente ed esterno

Questo argomento illustra come usare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione. Questo approccio funziona quando si dispone di più server Web perché ogni server Web può interagire con lo stesso repository di dati. Se i server Web smettono di `OnDisconnected` funzionare o l'applicazione viene riavviata, il metodo non viene chiamato. Pertanto, è possibile che il repository di dati dia a disposizione record per gli ID di connessione non più validi. Per pulire questi record orfani, è possibile invalidare qualsiasi connessione creata al di fuori di un intervallo di tempo rilevante per l'applicazione. Gli esempi in questa sezione includono un valore per il rilevamento di quando è stata creata la connessione, ma non mostrano come pulire i vecchi record perché è possibile eseguire questa operazione come processo in background.

### <a name="database"></a>Database

Negli esempi seguenti viene illustrato come mantenere le informazioni di connessione e utente in un database. È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli utilizzando Entity Framework. Questi modelli di entità corrispondono a tabelle e campi di database. La struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione.

Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a molte entità di connessione.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Quindi, dall'hub, è possibile tenere traccia dello stato di ogni connessione con il codice illustrato di seguito.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

L'esempio di archiviazione tabelle di Azure seguente è simile all'esempio di database. Non include tutte le informazioni necessarie per iniziare a usare il servizio di archiviazione tabelle di Azure.It does not include all of the information that you would need to get started with Azure Table Storage Service. Per informazioni, vedere [Come utilizzare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Nell'esempio seguente viene illustrata un'entità di tabella per l'archiviazione delle informazioni di connessione. Partiziona i dati in base al nome utente e identifica ogni entità in base all'ID di connessione, in modo che un utente possa avere più connessioni in qualsiasi momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Nell'hub, si tiene traccia dello stato della connessione di ogni utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
