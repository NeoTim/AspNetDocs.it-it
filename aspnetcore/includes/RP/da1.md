---
ms.openlocfilehash: e5565381f44480e2531925717ee7815da92d1ff5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030038"
---
# <a name="update-the-generated-pages"></a>Aggiornare le pagine generate

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale. Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere **Release Date** (due parole).

![App per i film aperta in Chrome con i dati sui film](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aggiornare il codice generato

Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
