---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilizzo di valori stringa di query per filtrare i dati con l'associazione di modelli e web form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 7ba95f24085c403c669bc5d6df4dee7c87fbd90a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59414244"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e web form

da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come passare un valore nella stringa di query e usare tale valore per recuperare i dati tramite l'associazione di modelli.
> 
> Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione, sarà:

1. Aggiungere una nuova pagina per visualizzare i corsi registrati per uno studente
2. Recuperare i corsi registrati dello studente selezionato in base a un valore nella stringa di query
3. Aggiungere un collegamento ipertestuale con un valore di stringa di query dalla visualizzazione griglia alla nuova pagina

I passaggi descritti in questa esercitazione sono piuttosto simili a quella usata in precedenza [esercitazione](sorting-paging-and-filtering-data.md) per filtrare gli studenti visualizzati in base alla selezione dell'utente in un elenco a discesa. In tale esercitazione, è stato usato il **controllo** attributo il metodo di selezione per specificare che il valore del parametro provenga da un controllo. In questa esercitazione si userà il **QueryString** attributo il metodo di selezione per specificare che il valore del parametro proviene dalla stringa di query.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Aggiungi nuova pagina per visualizzare i corsi di uno studente

Aggiungere un nuovo web form che utilizza la pagina master Site. master e denominare la pagina **corsi**.

Nel **Courses.aspx** , aggiungere una visualizzazione griglia per visualizzare i corsi per studente selezionato.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definire il metodo select

Nelle **Courses.aspx.cs**, si aggiungerà il metodo select con il nome specificato della visualizzazione griglia **SelectMethod** proprietà. In quel metodo, si sarà definire la query per il recupero dei corsi di uno studente e specificare che il parametro provenga da un valore di stringa di query con lo stesso nome come parametro.

In primo luogo, è necessario aggiungere quanto segue **usando** istruzioni.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Quindi, aggiungere il codice seguente a Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

L'attributo di stringa di query significa che un valore di stringa di query denominato ID studente viene automaticamente assegnato al parametro con questo metodo.

## <a name="add-hyperlink-with-query-string-value"></a>Aggiungi collegamento ipertestuale con valore di stringa di query

Nella visualizzazione griglia a Students.aspx, si aggiungerà un campo collegamento ipertestuale che collega alla nuova pagina di corsi. Il collegamento ipertestuale includeranno un valore di stringa di query con l'id dello studente.

In Students.aspx, aggiungere il campo seguente per le colonne della vista griglia sotto il campo per i crediti totali.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Eseguire l'applicazione e si noti che la visualizzazione griglia include ora il collegamento di corsi.

![Aggiungi collegamento ipertestuale](using-query-string-values-to-retrieve-data/_static/image1.png)

Quando si fa clic su uno dei collegamenti, si noterà corsi registrati dello studente.

![mostrare i corsi](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato aggiunto un collegamento con un valore di stringa di query. Tale valore di stringa di query è utilizzato per il valore del parametro del metodo select.

Entro i prossimi [esercitazione](adding-business-logic-layer.md), si passerà il codice da file code-behind in un livello di logica di business e un livello di accesso ai dati.

> [!div class="step-by-step"]
> [Precedente](integrating-jquery-ui.md)
> [Successivo](adding-business-logic-layer.md)
