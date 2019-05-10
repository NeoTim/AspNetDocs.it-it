---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Convalida con i validator di annotazione dei dati (VB) | Microsoft Docs
author: microsoft
description: È possibile sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione ASP.NET MVC. Informazioni su come usare i diversi tipi di convalida...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 00150575baabc659f7dd0c07349cde52105f6c8b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65117568"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Convalida con i validator di annotazione dei dati (VB)

by [Microsoft](https://github.com/microsoft)

> È possibile sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione ASP.NET MVC. Informazioni su come usare i diversi tipi di attributi di convalida e usarle in Microsoft Entity Framework.

In questa esercitazione descrive come usare i validator di annotazione dei dati per eseguire la convalida in un'applicazione ASP.NET MVC. Il vantaggio di usare i validator di annotazione dei dati è che consentono di eseguire la convalida mediante la semplice aggiunta di uno o più attributi, come richiesto o un attributo StringLength: per una proprietà di classe.

Prima di poter usare i validator di annotazione dei dati, è necessario scaricare lo strumento individuerebbe annotazioni dei dati. È possibile scaricare l'esempio di strumento di associazione di modello annotazioni dati dal sito Web CodePlex facendo [qui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

È importante comprendere che lo strumento individuerebbe annotazioni dei dati non è una parte ufficiale di Microsoft ASP.NET MVC framework. Anche se lo strumento individuerebbe annotazioni dei dati è stato creato dal team di Microsoft ASP.NET MVC, Microsoft non offre il supporto ufficiale del prodotto per lo strumento individuerebbe annotazioni dei dati descritti e usati in questa esercitazione.

## <a name="using-the-data-annotation-model-binder"></a>Usando lo strumento individuerebbe annotazione dei dati

Per usare lo strumento individuerebbe annotazioni dei dati in un'applicazione ASP.NET MVC, è necessario aggiungere un riferimento all'assembly Microsoft.Web.Mvc.DataAnnotations.dll e DataAnnotations assembly. Selezionare l'opzione di menu **progetto, Aggiungi riferimento**. Successivamente fare clic sui **esplorare** scheda e selezionare il percorso in cui è stato scaricato (e decompresso) l'esempio di Raccoglitore di modelli di annotazioni dei dati (vedere **figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: Aggiunta di un riferimento al Binder di modello di annotazioni dei dati ([fare clic per visualizzare l'immagine con dimensioni normali](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Selezionare sia la Microsoft.Web.Mvc.DataAnnotations.dll agli assembly e DataAnnotations e scegliere il **OK** pulsante.

È possibile usare l'assembly DataAnnotations incluso in .NET Framework Service Pack 1 con lo strumento individuerebbe annotazioni dei dati. È necessario usare la versione dell'assembly DataAnnotations incluso con il download di esempio dello strumento di associazione di dati le annotazioni del modello.

Infine, è necessario registrare il gestore di associazione di modelli di DataAnnotations nel file Global. asax. Aggiungere la riga di codice seguente all'applicazione\_gestore dell'evento Start () in modo che l'applicazione\_metodo Start () si presenta come segue:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Questa riga di codice registra il DataAnnotationsModelBinder come il raccoglitore dei modelli predefiniti per l'intera applicazione ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Usando gli attributi di Validator di annotazione dei dati

Quando si usa lo strumento individuerebbe annotazioni dei dati, si usano attributi di convalida per eseguire la convalida. Lo spazio dei nomi System.ComponentModel.DataAnnotations include gli attributi di convalida seguenti:

- Intervallo: consente di verificare se il valore di una proprietà è compreso tra un determinato intervallo di valori.
- RegularExpression: consente di verificare se il valore di una proprietà corrisponde a un modello di espressione regolare specificata.
- È necessario: consente di contrassegnare una proprietà in base alle esigenze.
- StringLength – consente di specificare una lunghezza massima per una proprietà di stringa.
- Convalida-classe di base per tutti gli attributi di convalida.

> [!NOTE] 
> 
> Se le esigenze di convalida non sono soddisfatte da una delle funzioni di convalida standard quindi avere sempre la possibilità di creare un attributo di validator personalizzato ereditando un nuovo attributo di convalida dall'attributo di convalida di base.

La classe di prodotto nel **listato 1** illustra come usare questi attributi di convalida. La proprietà Name, Description e UnitPrice sono contrassegnate come richiesto. La proprietà del nome deve avere una lunghezza di stringa contenente meno di 10 caratteri. Infine, la proprietà UnitPrice deve corrispondere un criterio di espressione regolare che rappresenta un importo di valuta.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listato 1**: Models\Product.vb

La classe di prodotto viene illustrato come utilizzare un attributo aggiuntivo: l'attributo DisplayName. L'attributo DisplayName consente di modificare il nome della proprietà quando la proprietà viene visualizzata in un messaggio di errore. Invece di visualizzare il messaggio di errore "il campo UnitPrice è obbligatorio" è possibile visualizzare il messaggio di errore "il campo del prezzo è obbligatorio".

> [!NOTE] 
> 
> Se si desidera personalizzare completamente il messaggio di errore visualizzato da un servizio validator è possibile assegnare un messaggio di errore personalizzato per la proprietà del validator messaggio di errore simile al seguente: `<Required(ErrorMessage:="This field needs a value!")>`

È possibile usare la classe di prodotto nel **listato 1** con l'azione del controller create () in **listato 2**. Questa azione del controller Visualizza di nuovo la visualizzazione Create quando lo stato del modello contiene eventuali errori.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listato 2**: Controllers\ProductController.vb

Infine, è possibile creare la vista **listato 3** facendo clic su azione create () e selezionando l'opzione di menu **Aggiungi visualizzazione**. Creare una visualizzazione fortemente tipizzata con la classe Product come la classe del modello. Selezionare **Create** nell'elenco a discesa contenuto visualizzazione (vedere **Figura2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: Aggiunta della visualizzazione Create

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listato 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Rimuovere il campo Id dal form crea generato per il **Aggiungi visualizzazione** l'opzione di menu. Poiché il campo Id corrisponde a una colonna Identity, non si vuole consentire agli utenti di immettere un valore per questo campo.

Se si invia il modulo per la creazione di un prodotto e non si immette i valori per i campi obbligatori, quindi i messaggi di errore di convalida nella **figura 3** vengono visualizzati.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: Campi obbligatori mancanti

Se si immette un importo di valuta non è valido, quindi il messaggio di errore in **figura 4** viene visualizzato.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: Importo di valuta non è valido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Uso di validator di annotazione dei dati con Entity Framework

Se si usa Microsoft Entity Framework per generare classi del modello di dati è possibile applicare gli attributi di convalida direttamente alle classi. Poiché la finestra di progettazione di Entity Framework genera le classi del modello, le modifiche apportate alle classi di modello verranno sovrascritto alla successiva che si apportano modifiche nella finestra di progettazione.

Se si desidera usare i validator con le classi generate da Entity Framework è necessario creare classi di dati di metadati. I validator è applicare alla classe di dati di metadati anziché applicare i validator alla classe effettiva.

Ad esempio, immaginare che hanno creato una classe di film mediante Entity Framework (vedere **figura 5**). Si supponga, inoltre, che si desidera rendere il titolo del film e direttore proprietà necessarie di proprietà. In tal caso, è possibile creare la classe parziale e metaclasse dati in **listato 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: Classe film generato da Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listato 4**: Models\Movie.vb

Il file nella **listato 4** contiene due classi denominate film e MovieMetaData. La classe di film è una classe parziale. Corrisponde alla classe parziale generata da Entity Framework che è contenuto nel file DataModel.Designer.vb.

Attualmente, .NET framework non supporta proprietà parziale. Pertanto, non è possibile applicare gli attributi di convalida per le proprietà della classe film definito nel file DataModel.Designer.vb applicando gli attributi di convalida per le proprietà della classe film definito nel file **listato 4**.

Si noti che la classe parziale del film è decorata con un attributo MetadataType che punta alla classe MovieMetaData. La classe MovieMetaData contiene proprietà proxy per le proprietà della classe di film.

Gli attributi di convalida vengono applicati alle proprietà della classe MovieMetaData. Le proprietà Title, Director e DateReleased tutti vengono contrassegnate come proprietà obbligatorie. La proprietà Director deve essere assegnata una stringa contenente meno di 5 caratteri. Infine, viene applicato l'attributo DisplayName alla proprietà DateReleased per visualizzare un messaggio di errore, ad esempio "il campo rilasciato data è obbligatorio". anziché l'errore "il campo DateReleased è obbligatorio".

> [!NOTE] 
> 
> Si noti che la proprietà nella classe MovieMetaData proxy non è necessario rappresentare gli stessi tipi di proprietà corrispondenti nella classe di film. Ad esempio, la proprietà Director è una proprietà di stringa della classe di film e proprietà di un oggetto della classe MovieMetaData.

La pagina **figura 6** illustra i messaggi di errore restituiti quando si immettono valori non validi per le proprietà di film.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: Uso di validator con Entity Framework ([fare clic per visualizzare l'immagine con dimensioni normali](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione ASP.NET MVC. Si è appreso come usare i diversi tipi di attributi di convalida come richiesto e StringLength attributi. Inoltre appreso come usare questi attributi quando si lavora sul Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Precedente](validating-with-a-service-layer-vb.md)
