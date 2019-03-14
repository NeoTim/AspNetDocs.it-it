---
ms.openlocfilehash: 3940675548ad7496aab9c720ee0b7fd512bfe029
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043458"
---

## <a name="test-the-app"></a>Eseguire il test dell'app

* Eseguire l'app e toccare il collegamento **Mvc film**.
* Toccare il collegamento **Crea nuovo** e creare un filmato.

  ![Creare una vista con i campi per genere, prezzo, data di rilascio e titolo](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* Potrebbe non essere possibile immettere separatori decimali o virgole nel campo `Price`. Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app. Per altre informazioni, vedere [https://github.com/aspnet/Docs/issues/4076](https://github.com/aspnet/Docs/issues/4076) e [Risorse aggiuntive](#additional-resources). Per il momento, immettere solo numeri interi come 10.

<a name="displayformatdatelocal"></a>

* In alcune impostazioni locali è necessario specificare il formato di data. Vedere il codice evidenziato di seguito.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

`DataAnnotations` verrà trattato più avanti nell'esercitazione.

Toccando **Crea** il modulo viene inviato al server, dove le informazioni relative al filmato vengono salvate in un database. L'applicazione reindirizza all'URL */Movies*, in cui vengono visualizzate le informazioni relative al filmato appena creato.

![Vista di Movies che visualizza l'elenco dei filmati appena creati](~/tutorials/first-mvc-app/adding-model/_static/h.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.
