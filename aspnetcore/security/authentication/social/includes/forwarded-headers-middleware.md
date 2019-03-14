---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130450"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Inoltra richiesta informazioni con un proxy o il bilanciamento del carico

Se l'app viene distribuito dietro un server proxy o un servizio di bilanciamento del carico, alcune delle informazioni della richiesta originale potrebbe essere inoltrato all'app nelle intestazioni della richiesta. Queste informazioni includono in genere lo schema di richiesta sicura (`https`), host e indirizzo IP del client. Le app non leggono automaticamente queste intestazioni di richiesta per individuare e usare le informazioni della richiesta originale.

Lo schema viene utilizzato nella generazione di collegamenti che interessa il flusso di autenticazione con provider esterni. Perdere lo schema sicuro (`https`) risultante nell'app per la generazione di URL di reindirizzamento non sicuro non corretto.

Usare Middleware delle intestazioni inoltrate per rendere disponibili per l'app per l'elaborazione della richiesta informazioni della richiesta originale.

Per altre informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.
