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
# <a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="bff60-105">Mapping degli utenti di SignalR alle connessioni</span><span class="sxs-lookup"><span data-stu-id="bff60-105">Mapping SignalR Users to Connections</span></span>

<span data-ttu-id="bff60-106"> di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bff60-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="bff60-107">In questo argomento viene illustrato come conservare le informazioni sugli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="bff60-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="bff60-108">Patrick Fletcher ha contribuito a scrivere questo argomento.</span><span class="sxs-lookup"><span data-stu-id="bff60-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="bff60-109">Versioni software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="bff60-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="bff60-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bff60-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="bff60-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bff60-111">.NET 4.5</span></span>
> - <span data-ttu-id="bff60-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="bff60-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="bff60-113">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="bff60-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="bff60-114">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="bff60-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="bff60-115">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="bff60-115">Questions and comments</span></span>
>
> <span data-ttu-id="bff60-116">Si prega di lasciare un feedback su come ti è piaciuto questo tutorial e cosa potremmo migliorare nei commenti in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="bff60-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bff60-117">Se avete domande che non sono direttamente correlati al tutorial, è possibile pubblicarli al [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bff60-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="bff60-118">Introduzione</span><span class="sxs-lookup"><span data-stu-id="bff60-118">Introduction</span></span>

<span data-ttu-id="bff60-119">Ogni client che si connette a un hub passa un ID di connessione univoco. È possibile recuperare questo `Context.ConnectionId` valore nella proprietà del contesto dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bff60-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="bff60-120">Se l'applicazione deve eseguire il mapping di un utente all'ID di connessione e rendere persistente il mapping, è possibile utilizzare uno degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bff60-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="bff60-121">Il provider di ID utente (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="bff60-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="bff60-122">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="bff60-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="bff60-123">Gruppo SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="bff60-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="bff60-124">[Archiviazione esterna permanente,](#database)ad esempio una tabella di database o un'archiviazione tabelle di AzurePermanent, external storage , such as a database table or Azure table storage</span><span class="sxs-lookup"><span data-stu-id="bff60-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="bff60-125">Ognuna di queste implementazioni viene illustrata in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="bff60-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="bff60-126">Utilizzare i `OnConnected` `OnDisconnected`metodi `OnReconnected` , e `Hub` della classe per tenere traccia dello stato della connessione utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="bff60-127">L'approccio migliore per l'applicazione dipende da:</span><span class="sxs-lookup"><span data-stu-id="bff60-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="bff60-128">Il numero di server Web che ospitano l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bff60-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="bff60-129">Se è necessario ottenere un elenco degli utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="bff60-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="bff60-130">Se è necessario rendere persistenti le informazioni di gruppo e utente al riavvio dell'applicazione o del server.</span><span class="sxs-lookup"><span data-stu-id="bff60-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="bff60-131">Se la latenza della chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="bff60-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="bff60-132">Nella tabella seguente viene illustrato quale approccio funziona per queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="bff60-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="bff60-133">Più di un server</span><span class="sxs-lookup"><span data-stu-id="bff60-133">More than one server</span></span> | <span data-ttu-id="bff60-134">Ottenere l'elenco degli utenti attualmente connessiGet list of currently connected users</span><span class="sxs-lookup"><span data-stu-id="bff60-134">Get list of currently connected users</span></span> | <span data-ttu-id="bff60-135">Rendere persistenti le informazioni dopo il riavvioPersist information after restarts</span><span class="sxs-lookup"><span data-stu-id="bff60-135">Persist information after restarts</span></span> | <span data-ttu-id="bff60-136">Prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="bff60-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="bff60-137">UserID Provider</span><span class="sxs-lookup"><span data-stu-id="bff60-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="bff60-138">In memoria</span><span class="sxs-lookup"><span data-stu-id="bff60-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="bff60-139">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="bff60-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="bff60-140">Permanente, esterna</span><span class="sxs-lookup"><span data-stu-id="bff60-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="bff60-141">Provider IUserID</span><span class="sxs-lookup"><span data-stu-id="bff60-141">IUserID provider</span></span>

<span data-ttu-id="bff60-142">Questa funzionalità consente agli utenti di specificare ciò che l'id utente è basato su un IRequest tramite una nuova interfaccia IUserIdProvider.This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="bff60-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="bff60-143">**The IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="bff60-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="bff60-144">Per impostazione predefinita, sarà presente un'implementazione che utilizza l'utente `IPrincipal.Identity.Name` come nome utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="bff60-145">Per modificare questa impostazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="bff60-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="bff60-146">Dall'interno di un hub, sarai in grado di inviare messaggi a questi utenti tramite la seguente API:</span><span class="sxs-lookup"><span data-stu-id="bff60-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="bff60-147">**Invio di un messaggio a un utente specifico**</span><span class="sxs-lookup"><span data-stu-id="bff60-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="bff60-148">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="bff60-148">In-memory storage</span></span>

<span data-ttu-id="bff60-149">Negli esempi seguenti viene illustrato come mantenere le informazioni di connessione e utente in un dizionario archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="bff60-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="bff60-150">Il dizionario `HashSet` utilizza un per archiviare l'ID di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="bff60-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="bff60-151">Ad esempio, un utente connesso tramite più dispositivi o più schede del browser avrebbe più di un ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="bff60-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="bff60-152">Se l'applicazione viene arrestata, tutte le informazioni andranno perse, ma verranno ripopolate man mano che gli utenti ristabiliscono le connessioni.</span><span class="sxs-lookup"><span data-stu-id="bff60-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="bff60-153">L'archiviazione in memoria non funziona se l'ambiente include più di un server Web perché ogni server avrebbe una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="bff60-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="bff60-154">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni.</span><span class="sxs-lookup"><span data-stu-id="bff60-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="bff60-155">La chiave per il HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="bff60-156">Nell'esempio successivo viene illustrato come utilizzare la classe di mapping delle connessioni da un hub.</span><span class="sxs-lookup"><span data-stu-id="bff60-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="bff60-157">L'istanza della classe viene archiviata in un nome `_connections`di variabile .</span><span class="sxs-lookup"><span data-stu-id="bff60-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="bff60-158">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="bff60-158">Single-user groups</span></span>

<span data-ttu-id="bff60-159">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="bff60-160">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="bff60-161">Se un utente dispone di più di una connessione, ogni ID di connessione viene aggiunto al gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="bff60-162">Non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="bff60-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="bff60-163">Questa azione viene eseguita automaticamente dal framework SignalR.</span><span class="sxs-lookup"><span data-stu-id="bff60-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="bff60-164">Nell'esempio seguente viene illustrato come implementare gruppi di utenti singoli.</span><span class="sxs-lookup"><span data-stu-id="bff60-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="bff60-165">Stoccaggio permanente ed esterno</span><span class="sxs-lookup"><span data-stu-id="bff60-165">Permanent, external storage</span></span>

<span data-ttu-id="bff60-166">Questo argomento illustra come usare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="bff60-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="bff60-167">Questo approccio funziona quando si dispone di più server Web perché ogni server Web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="bff60-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="bff60-168">Se i server Web smettono di `OnDisconnected` funzionare o l'applicazione viene riavviata, il metodo non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="bff60-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="bff60-169">Pertanto, è possibile che il repository di dati dia a disposizione record per gli ID di connessione non più validi.</span><span class="sxs-lookup"><span data-stu-id="bff60-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="bff60-170">Per pulire questi record orfani, è possibile invalidare qualsiasi connessione creata al di fuori di un intervallo di tempo rilevante per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bff60-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="bff60-171">Gli esempi in questa sezione includono un valore per il rilevamento di quando è stata creata la connessione, ma non mostrano come pulire i vecchi record perché è possibile eseguire questa operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="bff60-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="bff60-172">Database</span><span class="sxs-lookup"><span data-stu-id="bff60-172">Database</span></span>

<span data-ttu-id="bff60-173">Negli esempi seguenti viene illustrato come mantenere le informazioni di connessione e utente in un database.</span><span class="sxs-lookup"><span data-stu-id="bff60-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="bff60-174">È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli utilizzando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bff60-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="bff60-175">Questi modelli di entità corrispondono a tabelle e campi di database.</span><span class="sxs-lookup"><span data-stu-id="bff60-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="bff60-176">La struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bff60-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="bff60-177">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a molte entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="bff60-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="bff60-178">Quindi, dall'hub, è possibile tenere traccia dello stato di ogni connessione con il codice illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="bff60-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="bff60-179">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="bff60-179">Azure table storage</span></span>

<span data-ttu-id="bff60-180">L'esempio di archiviazione tabelle di Azure seguente è simile all'esempio di database.</span><span class="sxs-lookup"><span data-stu-id="bff60-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="bff60-181">Non include tutte le informazioni necessarie per iniziare a usare il servizio di archiviazione tabelle di Azure.It does not include all of the information that you would need to get started with Azure Table Storage Service.</span><span class="sxs-lookup"><span data-stu-id="bff60-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="bff60-182">Per informazioni, vedere [Come utilizzare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="bff60-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="bff60-183">Nell'esempio seguente viene illustrata un'entità di tabella per l'archiviazione delle informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="bff60-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="bff60-184">Partiziona i dati in base al nome utente e identifica ogni entità in base all'ID di connessione, in modo che un utente possa avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="bff60-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="bff60-185">Nell'hub, si tiene traccia dello stato della connessione di ogni utente.</span><span class="sxs-lookup"><span data-stu-id="bff60-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
