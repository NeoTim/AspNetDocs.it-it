---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: I dati (creazione di App Cloud funzionanti con Azure) di strategie di partizionamento | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 07a80767ca2def26c0252037a3b8b990abcbe1b3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051868"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="32dc4-104">Dati (creazione di App Cloud funzionanti con Azure) di strategie di partizionamento</span><span class="sxs-lookup"><span data-stu-id="32dc4-104">Data Partitioning Strategies (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="32dc4-105">dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="32dc4-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="32dc4-106">[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="32dc4-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="32dc4-107">Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="32dc4-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="32dc4-108">Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud.</span><span class="sxs-lookup"><span data-stu-id="32dc4-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="32dc4-109">Per informazioni sulla serie, vedere [capitolo prima](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="32dc4-109">For information about the series, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="32dc4-110">In precedenza è stato illustrato come è facile scalare il livello web di un'applicazione cloud, aggiungendo e rimuovendo i server web.</span><span class="sxs-lookup"><span data-stu-id="32dc4-110">Earlier we saw how easy it is to scale the web tier of a cloud application, by adding and removing web servers.</span></span> <span data-ttu-id="32dc4-111">Ma se sono tutti verificano lo stesso archivio dati, collo di bottiglia dell'applicazione viene spostato dal front-end al back-end e il livello dati è più difficile da ridimensionare.</span><span class="sxs-lookup"><span data-stu-id="32dc4-111">But if they're all hitting the same data store, your application's bottleneck moves from the front-end to the back-end, and the data tier is the hardest to scale.</span></span> <span data-ttu-id="32dc4-112">In questo capitolo illustra come è possibile apportare al livello dati scalabili tramite il partizionamento dei dati in più database relazionali o combinando l'archiviazione del database relazionale con altre opzioni di archiviazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="32dc4-112">In this chapter we look at how you can make your data tier scalable by partitioning data into multiple relational databases, or by combining relational database storage with other data storage options.</span></span>

<span data-ttu-id="32dc4-113">La configurazione di uno schema di partizionamento è migliore completato in anticipo per lo stesso motivo indicato in precedenza: è molto difficile modificarla strategia di archiviazione dei dati dopo che un'app è in fase di produzione.</span><span class="sxs-lookup"><span data-stu-id="32dc4-113">Setting up a partitioning scheme is best done up front for the same reason mentioned earlier: it's very difficult to change your data storage strategy after an app is in production.</span></span> <span data-ttu-id="32dc4-114">Se si pensa rigido fin dall'inizio approcci diversi, è possibile evitare "Twitter qualche" quando l'app si blocca o diventa inattiva per lungo tempo mentre si riorganizza i dati e codice di accesso ai dati dell'app.</span><span class="sxs-lookup"><span data-stu-id="32dc4-114">If you think hard up front about different approaches, you can avoid having a "Twitter moment" when your app crashes or goes down for a long time while you reorganize your app's data and data access code.</span></span>

## <a name="the-three-vs-of-data-storage"></a><span data-ttu-id="32dc4-115">Visual tre Studio dell'archivio dati</span><span class="sxs-lookup"><span data-stu-id="32dc4-115">The three Vs of data storage</span></span>

<span data-ttu-id="32dc4-116">Per determinare se è necessario una strategia di partizionamento e cosa dovrebbe esserlo, prendere in considerazione tre domande sui dati:</span><span class="sxs-lookup"><span data-stu-id="32dc4-116">In order to determine whether you need a partitioning strategy and what it should be, consider three questions about your data:</span></span>

- <span data-ttu-id="32dc4-117">Volume: la quantità di dati sarà necessario infine archiviare?</span><span class="sxs-lookup"><span data-stu-id="32dc4-117">Volume – How much data will you ultimately store?</span></span> <span data-ttu-id="32dc4-118">Due gigabyte?</span><span class="sxs-lookup"><span data-stu-id="32dc4-118">A couple gigabytes?</span></span> <span data-ttu-id="32dc4-119">Due centinaia gigabyte?</span><span class="sxs-lookup"><span data-stu-id="32dc4-119">A couple hundred gigabytes?</span></span> <span data-ttu-id="32dc4-120">Terabyte?</span><span class="sxs-lookup"><span data-stu-id="32dc4-120">Terabytes?</span></span> <span data-ttu-id="32dc4-121">Petabyte?</span><span class="sxs-lookup"><span data-stu-id="32dc4-121">Petabytes?</span></span>
- <span data-ttu-id="32dc4-122">Velocità: che cos'è la frequenza con cui i dati aumenterà?</span><span class="sxs-lookup"><span data-stu-id="32dc4-122">Velocity – What is the rate at which your data will grow?</span></span> <span data-ttu-id="32dc4-123">Si tratta di un'app interna che non è la generazione di una grande quantità di dati?</span><span class="sxs-lookup"><span data-stu-id="32dc4-123">Is it an internal app that isn't generating a lot of data?</span></span> <span data-ttu-id="32dc4-124">Un'applicazione esterna che ai clienti verranno caricata immagini e video in?</span><span class="sxs-lookup"><span data-stu-id="32dc4-124">An external app that customers will be uploading images and videos into?</span></span>
- <span data-ttu-id="32dc4-125">Diversi: il tipo di dati verranno archiviati?</span><span class="sxs-lookup"><span data-stu-id="32dc4-125">Variety – What type of data will you store?</span></span> <span data-ttu-id="32dc4-126">Relazionali, immagini, coppie chiave-valore, grafici basati su social network?</span><span class="sxs-lookup"><span data-stu-id="32dc4-126">Relational, images, key-value pairs, social graphs?</span></span>

<span data-ttu-id="32dc4-127">Se si ritiene che si intende presenta un elevato numero di volume, velocità o diverse, è necessario valutare con attenzione il tipo di schema di partizionamento ottimale permetterà all'app di scalare in modo efficiente ed efficace quando si espande, e per assicurarsi che non vengono eseguiti in eventuali colli di bottiglia.</span><span class="sxs-lookup"><span data-stu-id="32dc4-127">If you think you're going to have a lot of volume, velocity, or variety, you have to carefully consider what kind of partitioning scheme will best enable your app to scale efficiently and effectively as it grows, and to ensure you don't run into any bottlenecks.</span></span>

<span data-ttu-id="32dc4-128">Esistono sostanzialmente tre approcci di partizionamento:</span><span class="sxs-lookup"><span data-stu-id="32dc4-128">There are basically three approaches to partitioning:</span></span>

- <span data-ttu-id="32dc4-129">Partizionamento verticale</span><span class="sxs-lookup"><span data-stu-id="32dc4-129">Vertical partitioning</span></span>
- <span data-ttu-id="32dc4-130">Il partizionamento orizzontale</span><span class="sxs-lookup"><span data-stu-id="32dc4-130">Horizontal partitioning</span></span>
- <span data-ttu-id="32dc4-131">Partizionamento ibrido</span><span class="sxs-lookup"><span data-stu-id="32dc4-131">Hybrid partitioning</span></span>

## <a name="vertical-partitioning"></a><span data-ttu-id="32dc4-132">Partizionamento verticale</span><span class="sxs-lookup"><span data-stu-id="32dc4-132">Vertical partitioning</span></span>

<span data-ttu-id="32dc4-133">Porzioni verticale è, ad esempio suddividendo una tabella da colonne: un set di colonne entra in un archivio dati, e un altro set di colonne entra in un archivio dati diverso.</span><span class="sxs-lookup"><span data-stu-id="32dc4-133">Vertical portioning is like splitting up a table by columns: one set of columns goes into one data store, and another set of columns goes into a different data store.</span></span>

<span data-ttu-id="32dc4-134">Si supponga, ad esempio, che l'app archivia i dati sulle persone, incluse le immagini:</span><span class="sxs-lookup"><span data-stu-id="32dc4-134">For example, suppose my app stores data about people, including images:</span></span>

![Tabella di dati](data-partitioning-strategies/_static/image1.png)

<span data-ttu-id="32dc4-136">Quando rappresentare tali dati sotto forma di tabella ed esaminare i diversi tipi diversi di dati, si noterà che le tre colonne a sinistra sono dati di tipo stringa che possono essere archiviati in modo efficiente da un database relazionale, anche se le due colonne a destra sono essenzialmente le matrici di byte che c orte dai file di immagine.</span><span class="sxs-lookup"><span data-stu-id="32dc4-136">When you represent this data as a table and look at the different varieties of data, you can see that the three columns on the left have string data that can be efficiently stored by a relational database, while the two columns on the right are essentially byte arrays that come from image files.</span></span> <span data-ttu-id="32dc4-137">È possibile ai dati di file di immagine di archiviazione in un database relazionale e molti utenti eseguire l'operazione perché non si vuole salvare i dati nel file System.</span><span class="sxs-lookup"><span data-stu-id="32dc4-137">It's possible to storage image file data in a relational database, and a lot of people do that because they don't want to save the data to the file system.</span></span> <span data-ttu-id="32dc4-138">Potrebbero non avere un file system in grado di archiviare i necessari volumi di dati o potrebbe non desiderano gestire separato il backup e ripristino di sistema.</span><span class="sxs-lookup"><span data-stu-id="32dc4-138">They might not have a file system capable of storing the required volumes of data or they might not want to manage a separate back-up and restore system.</span></span> <span data-ttu-id="32dc4-139">Questo approccio funziona bene per i database locali e per piccole quantità di dati in database cloud.</span><span class="sxs-lookup"><span data-stu-id="32dc4-139">This approach works well for on-premises databases and for small amounts of data in cloud databases.</span></span> <span data-ttu-id="32dc4-140">Nell'ambiente locale, potrebbe essere più semplice consentire solo l'amministratore di database risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="32dc4-140">In the on-premises environment, it might be easier to just let the DBA take care of everything.</span></span>

<span data-ttu-id="32dc4-141">Ma in un database cloud, è relativamente costosa archiviazione e un volume elevato di immagini è stato possibile verificare le dimensioni del database di crescere oltre i limiti in corrispondenza del quale può funzionare in modo efficiente.</span><span class="sxs-lookup"><span data-stu-id="32dc4-141">But in a cloud database, storage is relatively expensive, and a high volume of images could make the size of the database grow beyond the limits at which it can operate efficiently.</span></span> <span data-ttu-id="32dc4-142">Si possono risolvere questi problemi tramite il partizionamento dei dati in verticale, ovvero che scelto l'archivio di dati più appropriato per ogni colonna della tabella di dati.</span><span class="sxs-lookup"><span data-stu-id="32dc4-142">You can address these problems by partitioning the data vertically, which means you choose the most appropriate data store for each column in your table of data.</span></span> <span data-ttu-id="32dc4-143">Ciò che potrebbe essere più adatti per questo esempio consiste nell'inserire i dati della stringa in un database relazionale e le immagini nell'archiviazione Blob.</span><span class="sxs-lookup"><span data-stu-id="32dc4-143">What might work best for this example is to put the string data in a relational database and the images in Blob storage.</span></span>

![Tabella di dati partizionata verticalmente](data-partitioning-strategies/_static/image2.png)

<span data-ttu-id="32dc4-145">L'archiviazione delle immagini nell'archiviazione Blob anziché un database è più pratico nel cloud rispetto a in un ambiente on-premises perché non è necessario preoccuparsi della configurazione di file server o la gestione di backup e ripristino dei dati archiviati all'esterno del database relazionale: tutti che viene eseguita automaticamente dal servizio di archiviazione Blob.</span><span class="sxs-lookup"><span data-stu-id="32dc4-145">Storing images in Blob storage instead of a database is more practical in the cloud than in an on-premises environment because you don't have to worry about setting up file servers or managing back-up and restore of data stored outside of the relational database: all that is handled for you automatically by the Blob storage service.</span></span>

<span data-ttu-id="32dc4-146">Si tratta dell'approccio di partizionamento è implementata nell'app Fix It e si cercheranno il codice che nel [capitolo archiviazione Blob](unstructured-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="32dc4-146">This is the partitioning approach we implemented in the Fix It app, and we'll look at the code for that in the [Blob storage chapter](unstructured-blob-storage.md).</span></span> <span data-ttu-id="32dc4-147">Senza questo schema di partizionamento e presupponendo una dimensione di immagine medio del 3 MB, l'app Fix It solo sarebbe in grado di archiviare circa 40.000 attività prima di raggiungere le dimensioni massime del database di 150 GB.</span><span class="sxs-lookup"><span data-stu-id="32dc4-147">Without this partitioning scheme, and assuming an average image size of 3 megabytes, the Fix It app would only be able to store about 40,000 tasks before hitting the maximum database size of 150 gigabytes.</span></span> <span data-ttu-id="32dc4-148">Dopo aver rimosso le immagini, il database può archiviare 10 volte superiore all'attività. è possibile andare molto più tempo prima è necessario preoccuparsi di implementare uno schema di partizionamento orizzontale.</span><span class="sxs-lookup"><span data-stu-id="32dc4-148">After removing the images, the database can store 10 times as many tasks; you can go much longer before you have to think about implementing a horizontal partitioning scheme.</span></span> <span data-ttu-id="32dc4-149">E se l'app viene scalata, le spese allunga di conseguenza più lento perché la maggior parte delle esigenze di archiviazione verranno nell'archiviazione Blob molto conveniente.</span><span class="sxs-lookup"><span data-stu-id="32dc4-149">And as the app scales, your expenses grow more slowly because the bulk of your storage needs are going into very inexpensive Blob storage.</span></span>

## <a name="horizontal-partitioning-sharding"></a><span data-ttu-id="32dc4-150">Partizionamento orizzontale (sharding)</span><span class="sxs-lookup"><span data-stu-id="32dc4-150">Horizontal partitioning (sharding)</span></span>

<span data-ttu-id="32dc4-151">Porzioni orizzontali è, ad esempio suddividendo una tabella dalle righe: un set di righe entra in un archivio dati, e un altro set di righe entra in un archivio dati diverso.</span><span class="sxs-lookup"><span data-stu-id="32dc4-151">Horizontal portioning is like splitting up a table by rows: one set of rows goes into one data store, and another set of rows goes into a different data store.</span></span>

<span data-ttu-id="32dc4-152">Dato lo stesso set di dati, un'altra opzione, è possibile archiviare diversi intervalli di nomi di clienti in database diversi.</span><span class="sxs-lookup"><span data-stu-id="32dc4-152">Given the same set of data, another option would be to store different ranges of customer names in different databases.</span></span>

![Tabella di dati partizionata orizzontalmente](data-partitioning-strategies/_static/image3.png)

<span data-ttu-id="32dc4-154">Si desidera prestare molta attenzione lo schema di partizionamento orizzontale per assicurarsi che i dati sono distribuiti uniformemente per evitare le aree sensibili.</span><span class="sxs-lookup"><span data-stu-id="32dc4-154">You want to be very careful about your sharding scheme to make sure that data is evenly distributed in order to avoid hot spots.</span></span> <span data-ttu-id="32dc4-155">Questo semplice esempio utilizzando la prima lettera del cognome non soddisfa questo requisito, perché molte persone hanno i cognomi che iniziano con determinate lettere comuni.</span><span class="sxs-lookup"><span data-stu-id="32dc4-155">This simple example using the first letter of the last name doesn't meet that requirement, because a lot of people have last names that start with certain common letters.</span></span> <span data-ttu-id="32dc4-156">Si raggiungeva prima del previsto perché alcuni database otterrebbe grandi mentre la maggior parte rimarrebbe piccole limitazioni relative alle dimensioni di tabella.</span><span class="sxs-lookup"><span data-stu-id="32dc4-156">You'd hit table size limitations earlier than you might expect because some databases would get very large while most would remain small.</span></span>

<span data-ttu-id="32dc4-157">Un lato negativo di partizionamento orizzontale è che potrebbe essere difficile eseguire query su tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="32dc4-157">A down side of horizontal partitioning is that it might be hard to do queries across all of the data.</span></span> <span data-ttu-id="32dc4-158">In questo esempio, una query sarebbe necessario disegnare a partire dal 26 fino a diversi database per ottenere tutti i dati archiviati dall'app.</span><span class="sxs-lookup"><span data-stu-id="32dc4-158">In this example, a query would have to draw from up to 26 different databases to get all of the data stored by the app.</span></span>

## <a name="hybrid-partitioning"></a><span data-ttu-id="32dc4-159">Partizionamento ibrido</span><span class="sxs-lookup"><span data-stu-id="32dc4-159">Hybrid partitioning</span></span>

<span data-ttu-id="32dc4-160">È possibile combinare il partizionamento orizzontale e verticale.</span><span class="sxs-lookup"><span data-stu-id="32dc4-160">You can combine vertical and horizontal partitioning.</span></span> <span data-ttu-id="32dc4-161">Ad esempio, nei dati di esempio è stato possibile archiviare le immagini nell'archiviazione Blob e partizionare orizzontalmente i dati della stringa.</span><span class="sxs-lookup"><span data-stu-id="32dc4-161">For example, in the example data you could store the images in Blob storage and horizontally partition the string data.</span></span>

![Ibrido di dati tabella partizionata](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a><span data-ttu-id="32dc4-163">Il partizionamento di un'applicazione di produzione</span><span class="sxs-lookup"><span data-stu-id="32dc4-163">Partitioning a production application</span></span>

<span data-ttu-id="32dc4-164">Concettualmente è facile vedere funzionamento uno schema di partizionamento, ma qualsiasi schema di partizionamento di aumentare la complessità del codice e introduce molte nuove complicazioni che devi affrontare.</span><span class="sxs-lookup"><span data-stu-id="32dc4-164">Conceptually it's easy to see how a partitioning scheme would work, but any partitioning scheme does increase code complexity and introduces many new complications that you have to deal with.</span></span> <span data-ttu-id="32dc4-165">Se stai spostando le immagini nell'archiviazione BLOB, cosa fare quando il servizio di archiviazione è inattivo?</span><span class="sxs-lookup"><span data-stu-id="32dc4-165">If you're moving images to blob storage, what do you do when the storage service is down?</span></span> <span data-ttu-id="32dc4-166">Come è possibile gestire la sicurezza blob?</span><span class="sxs-lookup"><span data-stu-id="32dc4-166">How do you handle blob security?</span></span> <span data-ttu-id="32dc4-167">Cosa accade se lo spazio di archiviazione blob e database non venga sincronizzato.</span><span class="sxs-lookup"><span data-stu-id="32dc4-167">What happens if the database and blob storage get out of sync?</span></span> <span data-ttu-id="32dc4-168">Se si ha di partizionamento orizzontale, come verranno gestiti eseguono query su tutti i database?</span><span class="sxs-lookup"><span data-stu-id="32dc4-168">If you're sharding, how will you handle querying across all of the databases?</span></span>

<span data-ttu-id="32dc4-169">Le complicazioni sono semplici da gestire, purché si intende per loro prima di passare all'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="32dc4-169">The complications are manageable so long as you're planning for them before you go to production.</span></span> <span data-ttu-id="32dc4-170">Molte persone che non desiderano che avevano in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="32dc4-170">Many people who didn't do that wish they had later.</span></span> <span data-ttu-id="32dc4-171">In Media il nostro team di Customer Advisory Team (CAT) Ottiene telefonate in panico su una volta al mese da parte dei clienti di App sono prendendo piede in modo significativo e operazioni non eseguite questa pianificazione.</span><span class="sxs-lookup"><span data-stu-id="32dc4-171">On average our Customer Advisory Team (CAT) team gets panicked phone calls about once a month from customers whose apps are taking off in a really big way, and they didn't do this planning.</span></span> <span data-ttu-id="32dc4-172">Scopro immancabilmente simile: "Help.</span><span class="sxs-lookup"><span data-stu-id="32dc4-172">And they say something like: "Help!</span></span> <span data-ttu-id="32dc4-173">Tutto ciò che è stato inserito in un unico archivio dati e di 45 giorni verrà esaurito lo spazio su tale!"</span><span class="sxs-lookup"><span data-stu-id="32dc4-173">I put everything in a single data store, and in 45 days I'm going to run out of space on it!"</span></span> <span data-ttu-id="32dc4-174">E se si ha una grande quantità di logica di business incorporata in modalità di accesso all'archivio dati e per i clienti che usano l'app, non sono previsti tempi buona per spostarsi in basso per un giorno, mentre esegue la migrazione.</span><span class="sxs-lookup"><span data-stu-id="32dc4-174">And if you have a lot of business logic built into how you access your data store and you have customers who are using your app, there's no good time to go down for a day while you migrate.</span></span> <span data-ttu-id="32dc4-175">Si finisce passare attraverso herculean sforzi per la partizione dei clienti i propri dati in tempo reale con senza tempi di inattività.</span><span class="sxs-lookup"><span data-stu-id="32dc4-175">We end up going through herculean efforts to help the customer partition their data on the fly with no down time.</span></span> <span data-ttu-id="32dc4-176">È molto appassionante e spaventosa e non si desidera essere coinvolto nel se è possibile evitarlo.</span><span class="sxs-lookup"><span data-stu-id="32dc4-176">It's very exciting and very scary, and not something you want to be involved in if you can avoid it!</span></span> <span data-ttu-id="32dc4-177">Valutando ciò fin dall'inizio e relativa integrazione in app per facilitare la vita molto se l'app cresce in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="32dc4-177">Thinking about this up front and integrating it into your app will make your life a lot easier if the app grows later.</span></span>

## <a name="summary"></a><span data-ttu-id="32dc4-178">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="32dc4-178">Summary</span></span>

<span data-ttu-id="32dc4-179">Un efficace schema di partizionamento è possibile abilitare le app cloud per la scalabilità fino a petabyte di dati nel cloud senza i colli di bottiglia.</span><span class="sxs-lookup"><span data-stu-id="32dc4-179">An effective partitioning scheme can enable your cloud app to scale to petabytes of data in the cloud without bottlenecks.</span></span> <span data-ttu-id="32dc4-180">E non devi pagare fin dall'inizio per le macchine di grandi dimensioni o un'infrastruttura completa come si farebbe se è stata usata l'app in un data center locale.</span><span class="sxs-lookup"><span data-stu-id="32dc4-180">And you don't have to pay up front for massive machines or extensive infrastructure as you might if you were running the app in an on-premises data center.</span></span> <span data-ttu-id="32dc4-181">Nel cloud è possibile è possibile aggiungere capacità in modo incrementale quando ti serve, e sta pagando solo per quanto si usa quando si usa lo.</span><span class="sxs-lookup"><span data-stu-id="32dc4-181">In the cloud you can you can incrementally add capacity as you need it, and you're only paying for as much as you're using when you use it.</span></span>

<span data-ttu-id="32dc4-182">Nel [capitolo successivo](unstructured-blob-storage.md) vedremo come l'app Fix It implementa il partizionamento verticale archiviando le immagini nell'archiviazione Blob.</span><span class="sxs-lookup"><span data-stu-id="32dc4-182">In the [next chapter](unstructured-blob-storage.md) we'll see how the Fix It app implements vertical partitioning by storing images in Blob storage.</span></span>

## <a name="resources"></a><span data-ttu-id="32dc4-183">Risorse</span><span class="sxs-lookup"><span data-stu-id="32dc4-183">Resources</span></span>

<span data-ttu-id="32dc4-184">Per altre informazioni sulle strategie di partizionamento, vedere le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="32dc4-184">For more information about partitioning strategies, see the following resources.</span></span>

<span data-ttu-id="32dc4-185">Documentazione:</span><span class="sxs-lookup"><span data-stu-id="32dc4-185">Documentation:</span></span>

- <span data-ttu-id="32dc4-186">[Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx).</span><span class="sxs-lookup"><span data-stu-id="32dc4-186">[Best Practices for the Design of Large-Scale Services on Windows Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx).</span></span> <span data-ttu-id="32dc4-187">White paper Mark Simms e Michael Thomassy.</span><span class="sxs-lookup"><span data-stu-id="32dc4-187">White paper by Mark Simms and Michael Thomassy.</span></span>
- <span data-ttu-id="32dc4-188">[Microsoft Patterns and Practices - Cloud Design Patterns](https://msdn.microsoft.com/library/dn568099.aspx).</span><span class="sxs-lookup"><span data-stu-id="32dc4-188">[Microsoft Patterns and Practices - Cloud Design Patterns](https://msdn.microsoft.com/library/dn568099.aspx).</span></span> <span data-ttu-id="32dc4-189">Vedere le indicazioni partizionamento dei dati, modello di partizionamento orizzontale.</span><span class="sxs-lookup"><span data-stu-id="32dc4-189">See Data Partitioning guidance, Sharding pattern.</span></span>

<span data-ttu-id="32dc4-190">Video</span><span class="sxs-lookup"><span data-stu-id="32dc4-190">Videos:</span></span>

- <span data-ttu-id="32dc4-191">[Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe).</span><span class="sxs-lookup"><span data-stu-id="32dc4-191">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="32dc4-192">Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms.</span><span class="sxs-lookup"><span data-stu-id="32dc4-192">Nine-part series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="32dc4-193">Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi.</span><span class="sxs-lookup"><span data-stu-id="32dc4-193">Presents high-level concepts and architectural principles in a very accessible and interesting way, with stories drawn from Microsoft Customer Advisory Team (CAT) experience with actual customers.</span></span> <span data-ttu-id="32dc4-194">Vedere la discussione partizionamento episodio 7.</span><span class="sxs-lookup"><span data-stu-id="32dc4-194">See the partitioning discussion in episode 7.</span></span>
- <span data-ttu-id="32dc4-195">[Big Data creazione: Lezioni apprese dai clienti di Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms illustra gli schemi di partizionamento, strategie di partizionamento orizzontale, come l'implementazione di partizionamento orizzontale e le federazioni di Database SQL, iniziando in corrispondenza di 19:49.</span><span class="sxs-lookup"><span data-stu-id="32dc4-195">[Building Big: Lessons learned from Windows Azure customers - Part I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms discusses partitioning schemes, sharding strategies, how to implement sharding, and SQL Database Federations, starting at 19:49.</span></span> <span data-ttu-id="32dc4-196">Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure.</span><span class="sxs-lookup"><span data-stu-id="32dc4-196">Similar to the Failsafe series but goes into more how-to details.</span></span>

<span data-ttu-id="32dc4-197">Codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="32dc4-197">Sample code:</span></span>

- <span data-ttu-id="32dc4-198">[Sviluppo di servizi in Windows Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="32dc4-198">[Cloud Service Fundamentals in Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="32dc4-199">Applicazione di esempio che include un database partizionato.</span><span class="sxs-lookup"><span data-stu-id="32dc4-199">Sample application that includes a sharded database.</span></span> <span data-ttu-id="32dc4-200">Per una descrizione dello schema di partizionamento orizzontale implementato, vedere [DAL: partizionamento orizzontale di RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) sul blog di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="32dc4-200">For a description of the sharding scheme implemented, see [DAL – Sharding of RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) on the Windows Azure blog.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32dc4-201">[Precedente](data-storage-options.md)
> [Successivo](unstructured-blob-storage.md)</span><span class="sxs-lookup"><span data-stu-id="32dc4-201">[Previous](data-storage-options.md)
[Next](unstructured-blob-storage.md)</span></span>