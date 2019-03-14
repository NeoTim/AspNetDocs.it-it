---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni persistenti di SignalR (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione di sicurezza in un'applicazione di SignalR,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: c4a2c9b4fa05e43ade98fb521c59f8645b43ffea
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034638"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="8cd32-104">Autenticazione e autorizzazione per le connessioni persistenti di SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="8cd32-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="8cd32-105">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8cd32-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="8cd32-106">In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente.</span><span class="sxs-lookup"><span data-stu-id="8cd32-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="8cd32-107">Per informazioni generali sull'integrazione della protezione in un'applicazione di SignalR, vedere [Introduzione alla sicurezza](index.md).</span><span class="sxs-lookup"><span data-stu-id="8cd32-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="8cd32-108">Imporre l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="8cd32-108">Enforce authorization</span></span>

<span data-ttu-id="8cd32-109">Per applicare le regole di autorizzazione quando si usa un' [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override di `AuthorizeRequest` (metodo).</span><span class="sxs-lookup"><span data-stu-id="8cd32-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="8cd32-110">Non è possibile usare il `Authorize` attributo con le connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="8cd32-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="8cd32-111">Il `AuthorizeRequest` viene chiamato dal Framework prima di ogni richiesta per verificare che l'utente è autorizzato a eseguire l'azione richiesta SignalR.</span><span class="sxs-lookup"><span data-stu-id="8cd32-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="8cd32-112">Il `AuthorizeRequest` metodo non viene chiamato dal client; in alternativa, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8cd32-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="8cd32-113">L'esempio seguente viene illustrato come limitare le richieste per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="8cd32-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="8cd32-114">È possibile aggiungere qualsiasi logica di autorizzazione personalizzato nel metodo AuthorizeRequest; ad esempio, verifica se un utente appartiene a un particolare ruolo.</span><span class="sxs-lookup"><span data-stu-id="8cd32-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
