---
uid: signalr/overview/security/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni persistenti di SignalR | Microsoft Docs
author: bradygaster
description: In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione di sicurezza in un'applicazione di SignalR,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65113533"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Autenticazione e autorizzazione per le connessioni persistenti di SignalR

dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione della protezione in un'applicazione di SignalR, vedere [Introduzione alla sicurezza](introduction-to-security.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).

## <a name="enforce-authorization"></a>Imporre l'autorizzazione

Per applicare le regole di autorizzazione quando si usa un' [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override di `AuthorizeRequest` (metodo). Non è possibile usare il `Authorize` attributo con le connessioni permanenti. Il `AuthorizeRequest` viene chiamato dal Framework prima di ogni richiesta per verificare che l'utente è autorizzato a eseguire l'azione richiesta SignalR. Il `AuthorizeRequest` metodo non viene chiamato dal client; in alternativa, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.

L'esempio seguente viene illustrato come limitare le richieste per gli utenti autenticati.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

È possibile aggiungere qualsiasi logica di autorizzazione personalizzato nel metodo AuthorizeRequest; ad esempio, verifica se un utente appartiene a un particolare ruolo.
