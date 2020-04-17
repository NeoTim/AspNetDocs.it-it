---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Convalida con i validatori di annotazione dei dati (VB) Documenti Microsoft
author: rick-anderson
description: Sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione MVC ASP.NET. Scopri come utilizzare i diversi tipi di validatore...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: cabdf6dab9e5de53a45adcf126705533fca02de7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542638"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Convalida con i validator di annotazione dei dati (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione MVC ASP.NET. Informazioni su come utilizzare i diversi tipi di attributi di convalida e utilizzarli in Microsoft Entity Framework.

In questa esercitazione si apprenderà come usare i validator di annotazione dei dati per eseguire la convalida in un'applicazione MVC ASP.NET. Il vantaggio dell'utilizzo dei validator di annotazione dei dati è che consentono di eseguire la convalida semplicemente aggiungendo uno o più attributi, ad esempio l'attributo Required o StringLength, a una proprietà della classe.

Prima di poter utilizzare i validator di annotazione dei dati, è necessario scaricare il raccoglitore del modello di annotazioni dei dati. È possibile scaricare l'esempio Data Annotations Model Binder dal sito Web CodePlex facendo clic [qui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

È importante comprendere che il raccoglitore di modelli di annotazioni dei dati non è una parte ufficiale del framework Microsoft ASP.NET MVC. Anche se il raccoglitore di modelli di annotazioni dei dati è stato creato dal team di Microsoft ASP.NET MVC, Microsoft non offre il supporto ufficiale del prodotto per il raccoglitore di modelli di annotazioni dei dati descritto e utilizzato in questa esercitazione.

## <a name="using-the-data-annotation-model-binder"></a>Utilizzo del gestore di associazione del modello di annotazione dei datiUsing the Data Annotation Model Binder

Per utilizzare il raccoglitore di modelli di annotazioni dei dati in un'applicazione MVC ASP.NET, è innanzitutto necessario aggiungere un riferimento all'assembly Microsoft.Web.Mvc.DataAnnotations.dll e all'assembly System.ComponentModel.DataAnnotations.dll. Selezionare l'opzione di menu **Progetto, Aggiungi riferimento**. Fare quindi clic sulla scheda **Sfoglia** e passare al percorso in cui è stato scaricato (e decompresso) l'esempio Data Annotations Model Binder (vedere **la Figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1:** aggiunta di un riferimento al raccoglitore di modelli di annotazioni dei dati[(Fare clic per visualizzare l'immagine a dimensioni intere](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Selezionare l'assembly Microsoft.Web.Mvc.DataAnnotations.dll e l'assembly System.ComponentModel.DataAnnotations.dll e fare clic sul pulsante **OK.**

Non è possibile utilizzare l'assembly System.ComponentModel.DataAnnotations.dll incluso in .NET Framework Service Pack 1 con il raccoglitore del modello di annotazioni dei dati. È necessario utilizzare la versione dell'assembly System.ComponentModel.DataAnnotations.dll inclusa nel download dell'esempio di gestore di associazione del modello di annotazioni dei dati.

Infine, è necessario registrare il Raccoglitore di modelli DataAnnotations nel file Global.asax. Aggiungete la seguente riga\_di codice al gestore\_eventi Application Start() in modo che il metodo Application Start() sia simile al seguente:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Questa riga di codice registra DataAnnotationsModelBinder come gestore di associazione del modello predefinito per l'intera ASP.NET applicazione MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Utilizzo degli attributi di convalida dell'annotazione dei datiUsing the Data Annotation Validator Attributes

Quando si utilizza il Raccoglitore modelli di annotazioni dei dati, si utilizzano gli attributi di convalida per eseguire la convalida. Lo spazio dei nomi System.ComponentModel.DataAnnotations include i seguenti attributi di convalida:

- Intervallo: consente di convalidare se il valore di una proprietà rientra in un intervallo di valori specificato.
- RegularExpression: consente di convalidare se il valore di una proprietà corrisponde a un modello di espressione regolare specificato.
- Obbligatorio: consente di contrassegnare una proprietà in base alle esigenze.
- StringLength : Consente di specificare una lunghezza massima per una proprietà stringa.
- Validation: classe base per tutti gli attributi di convalida.

> [!NOTE] 
> 
> Se le esigenze di convalida non sono soddisfatte da uno dei validator standard, è sempre possibile creare un attributo validator personalizzato ereditando un nuovo attributo validator dall'attributo di convalida di base.

La classe Product nel **listato 1** illustra come utilizzare questi attributi validator. Le proprietà Name, Description e UnitPrice sono contrassegnate come obbligatorie. Il Name proprietà deve avere una lunghezza di stringa inferiore a 10 caratteri. Infine, il UnitPrice proprietà deve corrispondere a un modello di espressione regolare che rappresenta un importo in valuta.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listato 1**: Modelli- Prodotto.vb

La classe Product illustra come utilizzare un attributo aggiuntivo: l'attributo DisplayName. L'attributo DisplayName consente di modificare il nome della proprietà quando la proprietà viene visualizzata in un messaggio di errore. Anziché visualizzare il messaggio di errore "Il campo UnitPrice è obbligatorio" è possibile visualizzare il messaggio di errore "Il campo Prezzo è obbligatorio".

> [!NOTE] 
> 
> Se si desidera personalizzare completamente il messaggio di errore visualizzato da un validator, è possibile assegnare un messaggio di errore personalizzato alla proprietà ErrorMessage del validator in questo modo:`<Required(ErrorMessage:="This field needs a value!")>`

È possibile utilizzare la classe Product nel **listato 1** con l'azione Create() controller nel **listato 2**. Questa azione del controller visualizza nuovamente la visualizzazione Crea quando lo stato del modello contiene errori.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listato 2**: Controller-ProductController.vb

Infine, potete creare la vista nel **listato 3** facendo clic con il pulsante destro del mouse sull'azione Crea() e selezionando l'opzione di menu **Aggiungi vista**. Creare una visualizzazione fortemente tipizzata con la classe Product come classe del modello. Selezionare **Crea** dall'elenco a discesa del contenuto della visualizzazione (vedere **la figura 2).**

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: Aggiunta della vista Crea

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listato 3**: Visualizzazioni del prodotto e Create.aspx

> [!NOTE] 
> 
> Rimuovere il campo Id dal modulo Crea generato dall'opzione di menu **Aggiungi visualizzazione.** Poiché il campo Id corrisponde a una colonna Identity, non si desidera consentire agli utenti di immettere un valore per questo campo.

Se si invia il modulo per la creazione di un prodotto e non si immettono valori per i campi obbligatori, vengono visualizzati i messaggi di errore di convalida nella **Figura 3.**

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: Campi obbligatori mancanti

Se si immette un importo in valuta non valido, viene visualizzato il messaggio di errore nella **Figura 4.**

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: Importo in valuta non valido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Utilizzo di convalida dell'annotazione dei dati con Entity FrameworkUsing Data Annotation Validators with the Entity Framework

Se si utilizza Microsoft Entity Framework per generare le classi del modello di dati, non è possibile applicare gli attributi di convalida direttamente alle classi. Poiché Entity Framework Designer genera le classi del modello, tutte le modifiche apportate alle classi del modello verranno sovrascritte alla successiva esecuzione di eventuali modifiche nella finestra di progettazione.

Se si desidera utilizzare i validator con le classi generate da Entity Framework, è necessario creare classi di metadati. Si applicano i validator alla classe di metadati anziché applicare i validator alla classe effettiva.

Si supponga, ad esempio, di aver creato una classe Movie utilizzando Entity Framework (vedere **la Figura 5**). Si supponga, inoltre, che si desidera rendere il titolo del film e Director proprietà necessarie proprietà. In tal caso, è possibile creare la classe parziale e la classe di metadati nel **listato 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5:** classe Movie generata da Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listato 4**: Modelli,Film.vb

Il file nel **listato 4** contiene due classi denominate Movie e MovieMetaData. La classe Movie è una classe parziale. Corrisponde alla classe parziale generata da Entity Framework contenuto nel file DataModel.Designer.vb.

Attualmente, il framework .NET non supporta le proprietà parziali. Pertanto, non è possibile applicare gli attributi di convalida alle proprietà della classe Movie definita nel file DataModel.Designer.vb applicando gli attributi di convalida alle proprietà della classe Movie definite nel file nel **listato 4**.

Si noti che la classe parziale Movie è decorata con un MetadataType attributo che punta alla classe MovieMetaData. La classe MovieMetaData contiene le proprietà proxy per le proprietà della classe Movie.

Gli attributi di convalida vengono applicati alle proprietà della classe MovieMetaData. Le proprietà Title, Director e DateReleased sono tutte contrassegnate come proprietà obbligatorie. Alla proprietà Director deve essere assegnata una stringa contenente meno di 5 caratteri. Infine, l'attributo DisplayName viene applicato alla proprietà DateReleased per visualizzare un messaggio di errore del tipo "Il campo Data rilascio è obbligatorio". anziché l'errore "Il campo DateReleased è obbligatorio".

> [!NOTE] 
> 
> Si noti che non è necessario che le proprietà proxy nella classe MovieMetaData rappresentino gli stessi tipi delle proprietà corrispondenti nella classe Movie. Ad esempio, la proprietà Director è una proprietà stringa nella classe Movie e una proprietà object nella classe MovieMetaData.

La pagina in **Figura 6** illustra i messaggi di errore restituiti quando si immettono valori non validi per il Movie proprietà.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: Utilizzo di validator con Entity Framework ([Fare clic per visualizzare l'immagine a dimensioni intere](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione MVC ASP.NET. Si è appreso come utilizzare i diversi tipi di attributi di convalida, ad esempio gli attributi Required e StringLength. Si è inoltre appreso come utilizzare questi attributi quando si lavora con Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Indietro](validating-with-a-service-layer-vb.md)
