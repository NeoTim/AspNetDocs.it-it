---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Aggiunta di un nuovo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 7018de3eab9d7cced72c76d0b74a79f7c8154f9d
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045143"
---
# <a name="adding-a-new-field"></a>Aggiunta di un nuovo campo

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

In questa sezione si utilizzerà Migrazioni Code First di Entity Framework per eseguire la migrazione di alcune modifiche alle classi del modello in modo da applicare la modifica al database.

Per impostazione predefinita, quando si utilizza Entity Framework Code First per creare automaticamente un database, come in precedenza in questa esercitazione, Code First aggiunge una tabella al database per tenere traccia dell'eventuale sincronizzazione dello schema del database con le classi del modello da cui è stato generato. Se non sono sincronizzati, il Entity Framework genera un errore. In questo modo è più semplice tenere traccia dei problemi in fase di sviluppo che potrebbero essere individuati (per errori nascosti) in fase di esecuzione.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Impostazione Migrazioni Code First per le modifiche al modello

Passare a Esplora soluzioni. Fare clic con il pulsante destro del mouse sul file *Movies. MDF* e selezionare **Elimina** per rimuovere il database Movies. Se non viene visualizzato il file *Movies. MDF* , fare clic sull'icona **Mostra tutti i file** mostrata sotto nella struttura rossa.

![](adding-a-new-field/_static/image1.png)

Compilare l'applicazione e verificare che non siano presenti errori.

Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet** e quindi su **console di gestione pacchetti**.

![Aggiungi Pack Man](adding-a-new-field/_static/image2.png)

Nella finestra **console di gestione pacchetti** al `PM>` prompt dei comandi immettere

Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext

![](adding-a-new-field/_static/image3.png)

Il comando **Enable-Migrations** (illustrato in precedenza) consente di creare un file *Configuration.cs* in una nuova cartella *Migrations* .

![](adding-a-new-field/_static/image4.png)

Visual Studio apre il file *Configuration.cs* . Sostituire il `Seed` metodo nel file *Configuration.cs* con il codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Posizionare il puntatore del mouse sulla linea rossa ondulata sotto `Movie` e fare clic su `Show Potential Fixes` e quindi su **using** **MvcMovie. Models;**

![](adding-a-new-field/_static/image5.png)

Questa operazione consente di aggiungere l'istruzione using seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Migrazioni Code First chiama il `Seed` metodo dopo ogni migrazione (ovvero, chiamando **Update-database** nella console di gestione pacchetti) e questo metodo aggiorna le righe che sono già state inserite o le inserisce se non esistono ancora.
> 
> Il metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) nel codice seguente esegue un'operazione "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Poiché il metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) viene eseguito con ogni migrazione, non è possibile inserire semplicemente i dati perché le righe che si sta tentando di aggiungere saranno già presenti dopo la prima migrazione che crea il database. L'operazione "[Upsert](http://en.wikipedia.org/wiki/Upsert)" impedisce gli errori che si verificano se si tenta di inserire una riga già esistente, ma viene eseguito l'override di eventuali modifiche apportate ai dati durante il test dell'applicazione. Con i dati di test in alcune tabelle potrebbe non essere necessario eseguire questa operazione: in alcuni casi, quando si modificano i dati durante il test si desidera che le modifiche rimangano dopo gli aggiornamenti del database. In tal caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già.   
> 
> Il primo parametro passato al metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) specifica la proprietà da utilizzare per verificare se una riga esiste già. Per i dati del film di test da fornire, `Title` è possibile usare la proprietà per questo scopo, perché ogni titolo dell'elenco è univoco:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Questo codice presuppone che i titoli siano univoci. Se si aggiunge manualmente un titolo duplicato, alla successiva esecuzione di una migrazione verrà generata l'eccezione seguente.   
> 
> *La sequenza contiene più di un elemento*  
> 
> Per ulteriori informazioni sul metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , vedere [prestare attenzione al metodo AddOrUpdate di EF 4,3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).

**Premere CTRL + MAIUSC + B per compilare il progetto.** I passaggi seguenti avranno esito negativo se non si compila a questo punto.

Il passaggio successivo consiste nel creare una `DbMigration` classe per la migrazione iniziale. Questa migrazione crea un nuovo database, motivo per cui è stato eliminato il file *Movie. MDF* in un passaggio precedente.

Nella finestra **console di gestione pacchetti** immettere il comando `add-migration Initial` per creare la migrazione iniziale. Il nome "Initial" è arbitrario e viene usato per assegnare un nome al file di migrazione creato.

![](adding-a-new-field/_static/image6.png)

Migrazioni Code First crea un altro file di classe nella cartella *Migrations* (con il nome *{datestamp} \_ Initial.cs* ) e questa classe contiene il codice che crea lo schema del database. Il nome del file di migrazione è preceduto da un timestamp per facilitare l'ordinamento. Esaminare il file * \_ Initial.cs {datestamp}* che contiene le istruzioni per creare la `Movies` tabella per il database di film. Quando si aggiorna il database nelle istruzioni seguenti, questo file *{datestamp} \_ Initial.cs* verrà eseguito e creerà lo schema del database. Viene quindi eseguito il metodo **Seed** per popolare il database con i dati di test.

Nella **console di gestione pacchetti**immettere il comando `update-database` per creare il database ed eseguire il `Seed` metodo.

![](adding-a-new-field/_static/image7.png)

Se viene generato un errore che indica che una tabella esiste già e non può essere creata, è probabile che l'applicazione sia stata eseguita dopo l'eliminazione del database e prima dell'esecuzione `update-database` . In tal caso, eliminare nuovamente il file *Movies. MDF* e ripetere il `update-database` comando. Se viene comunque ricevuto un errore, eliminare la cartella migrazioni e il contenuto, quindi iniziare con le istruzioni nella parte superiore di questa pagina, ovvero eliminare il file *Movies. MDF* , quindi passare a Enable-Migrations. Se viene comunque visualizzato un errore, aprire Esplora oggetti di SQL Server e rimuovere il database dall'elenco.

Eseguire l'applicazione e passare all'URL */Movies* . Vengono visualizzati i dati di inizializzazione.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere una nuova `Rating` proprietà alla `Movie` classe esistente. Aprire il file *Models\Movie.cs* e aggiungere la `Rating` proprietà come questa:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La classe completa ha `Movie` ora un aspetto simile al codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compilare l'applicazione (CTRL + MAIUSC + B).

Poiché è stato aggiunto un nuovo campo alla `Movie` classe, è necessario aggiornare anche l' *elenco bianco* di binding in modo che questa nuova proprietà venga inclusa. Aggiornare l' `bind` attributo per `Create` i `Edit` metodi di azione e in modo da includere la `Rating` proprietà:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.

Aprire il file *\Views\Movies\Index.cshtml* e aggiungere un' `<th>Rating</th>` intestazione di colonna immediatamente dopo la colonna **Price** . Aggiungere quindi una `<td>` colonna accanto alla fine del modello per eseguire il rendering del `@item.Rating` valore. Di seguito è riportato il modello di visualizzazione *index. cshtml* aggiornato:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Successivamente, aprire il file *\Views\Movies\Create.cshtml* e aggiungere il `Rating` campo con il markup evidenziato seguente. Viene eseguito il rendering di una casella di testo in modo da poter specificare una classificazione quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` Proprietà.

Eseguire l'applicazione e passare all'URL */Movies* . Quando si esegue questa operazione, tuttavia, verrà visualizzato uno degli errori seguenti:

![](adding-a-new-field/_static/image9.png)  
  
Il modello che supporta il contesto ' MovieDBContext ' è stato modificato dopo la creazione del database. Si consiglia di utilizzare Migrazioni Code First per aggiornare il database di ( https://go.microsoft.com/fwlink/?LinkId=238269) .

![](adding-a-new-field/_static/image10.png)

Questo errore viene visualizzato perché la classe del `Movie` modello aggiornata nell'applicazione è ora diversa dallo schema della `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme. Tuttavia, il lato negativo è che si perdono i dati esistenti nel database, quindi *non* si vuole usare questo approccio in un database di produzione. Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore. Per altre informazioni sugli inizializzatori di database Entity Framework, vedere l' [esercitazione su MVC/Entity Framework di ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.
3. Usare Migrazioni Code First per aggiornare lo schema del database.

Per questa esercitazione si userà Migrazioni Code First.

Aggiornare il metodo seed in modo da fornire un valore per la nuova colonna. Aprire il file Migrations\Configuration.cs e aggiungere un campo rating a ogni oggetto Movie.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** e immettere il comando seguente:

`add-migration Rating`

Il `add-migration` comando indica al Framework di migrazione di esaminare il modello di film corrente con lo schema del database del film corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello. La *classificazione* dei nomi è arbitraria e viene usata per assegnare un nome al file di migrazione. È utile usare un nome significativo per il passaggio di migrazione.

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMigration` classe derivata e nel `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compilare la soluzione, quindi immettere il `update-database` comando nella finestra **console di gestione pacchetti** .

Nell'immagine seguente viene illustrato l'output nella finestra della **console di gestione pacchetti** (la *classificazione* di data di inizio in sospeso sarà diversa).

![](adding-a-new-field/_static/image11.png)

Eseguire di nuovo l'applicazione e passare all'URL/Movies. È possibile visualizzare il nuovo campo rating.

![](adding-a-new-field/_static/image12.png)

Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Fare clic su **Crea**. Il nuovo film, incluso il rating, viene ora visualizzato nell'elenco dei film:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Ora che il progetto usa le migrazioni, non è necessario eliminare il database quando si aggiunge un nuovo campo o in caso contrario aggiornare lo schema. Nella sezione successiva verranno apportate ulteriori modifiche allo schema e verranno utilizzate le migrazioni per aggiornare il database.

È inoltre necessario aggiungere il `Rating` campo ai modelli di visualizzazione modifica, dettagli ed Elimina.

È possibile immettere di nuovo il comando "Update-database" nella finestra della **console di gestione pacchetti** e non verrà eseguito alcun codice di migrazione perché lo schema corrisponde al modello. Tuttavia, l'esecuzione di "Update-database" eseguirà di `Seed` nuovo il metodo e, se è stato modificato uno dei dati di inizializzazione, le modifiche andranno perse perché il `Seed` Metodo Upsert i dati. Per altre informazioni sul `Seed` metodo, vedere l'esercitazione più popolare su [ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra.

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. È stato inoltre illustrato un modo per popolare un database appena creato con dati di esempio, in modo da poter provare gli scenari. Questa è stata una rapida introduzione ai Code First, vedere [creazione di un modello di dati Entity Framework per un'applicazione MVC ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) per un'esercitazione più completa sull'argomento. Si osserverà ora come aggiungere una logica di convalida più completa alle classi del modello e abilitare l'applicazione di alcune regole business.

> [!div class="step-by-step"]
> [Precedente](adding-search.md) 
>  [Avanti](adding-validation.md)
