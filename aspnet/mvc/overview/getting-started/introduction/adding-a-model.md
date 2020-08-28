---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 453a55bd9f16c7b33525de18bc49493d22776f47
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045117"
---
# <a name="adding-a-model"></a>Aggiunta di un modello

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database. Queste classi saranno la &quot; &quot; parte del modello dell'app MVC ASP.NET.

Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come [Entity Framework](https://docs.microsoft.com/ef/) per definire e utilizzare queste classi di modelli. Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*. Code First consente di creare oggetti modello scrivendo classi semplici. Questi sono anche noti come classi POCO, da &quot; oggetti CLR obsoleti. &quot; È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido. Se è necessario creare prima il database, è comunque possibile seguire questa esercitazione per informazioni sullo sviluppo di app MVC e EF. È quindi possibile seguire l'esercitazione sull' [impalcatura](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) di Tom Fizmakens ASP.NET, che illustra il primo approccio al database.

## <a name="adding-model-classes"></a>Aggiunta di classi di modelli

In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Immettere il nome della *classe* &quot; Movie &quot; .

Aggiungere le cinque proprietà seguenti alla `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Verrà usata la `Movie` classe per rappresentare i film in un database. Ogni istanza di un `Movie` oggetto corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della `Movie` classe eseguirà il mapping a una colonna nella tabella.

Nota: per usare System. Data. Entity e la classe correlata, è necessario installare il [pacchetto NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/). Per altre istruzioni, seguire il collegamento.

Nello stesso file aggiungere la `MovieDBContext` classe seguente:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

La `MovieDBContext` classe rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle `Movie` istanze di classe in un database. `MovieDBContext`Deriva dalla `DbContext` classe di base fornita dal Entity Framework.

Per poter fare riferimento a `DbContext` e `DbSet` , è necessario aggiungere l' `using` istruzione seguente all'inizio del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

È possibile eseguire questa operazione aggiungendo manualmente l'istruzione using oppure è possibile passare il mouse sulle righe rosse ondulate, fare clic `Show potential fixes` e fare clic su `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: `using` sono state rimosse diverse istruzioni inutilizzate. Visual Studio mostrerà le dipendenze inutilizzate come grigio. È possibile rimuovere le dipendenze non utilizzate passando il mouse sulle dipendenze grigie, fare clic su `Show potential fixes` **Rimuovi using inutilizzate.**

![](adding-a-model/_static/image3.png)

È stato infine aggiunto un modello (il M in MVC). Nella sezione successiva verrà utilizzata la stringa di connessione del database.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md) 
>  [Avanti](creating-a-connection-string.md)
