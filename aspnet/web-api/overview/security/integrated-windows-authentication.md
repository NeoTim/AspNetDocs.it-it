---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticazione integrata di Windows | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo dell'autenticazione integrata di Windows in API Web ASP.NET.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621744"
---
# <a name="integrated-windows-authentication"></a><span data-ttu-id="a0450-103">Autenticazione integrata di Windows</span><span class="sxs-lookup"><span data-stu-id="a0450-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="a0450-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a0450-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a0450-105">Autenticazione integrata di Windows consente agli utenti di accedere con le proprie credenziali di Windows, utilizzando Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="a0450-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="a0450-106">Il client invia le credenziali nell'intestazione dell'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a0450-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="a0450-107">L'autenticazione di Windows è più adatta per un ambiente Intranet.</span><span class="sxs-lookup"><span data-stu-id="a0450-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="a0450-108">Per altre informazioni, vedere [Autenticazione di Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="a0450-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="a0450-109">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="a0450-109">Advantages</span></span> | <span data-ttu-id="a0450-110">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="a0450-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="a0450-111">-Integrato in IIS.</span><span class="sxs-lookup"><span data-stu-id="a0450-111">- Built into IIS.</span></span> <span data-ttu-id="a0450-112">-Non invia le credenziali utente nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="a0450-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="a0450-113">-Se il computer client appartiene al dominio, ad esempio un'applicazione Intranet, l'utente non deve immettere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a0450-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="a0450-114">-Sconsigliato per le applicazioni Internet.</span><span class="sxs-lookup"><span data-stu-id="a0450-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="a0450-115">-Richiede il supporto Kerberos o NTLM nel client.</span><span class="sxs-lookup"><span data-stu-id="a0450-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="a0450-116">-Il client deve trovarsi nel dominio Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a0450-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="a0450-117">Se l'applicazione è ospitata in Azure e si dispone di un dominio di Active Directory locale, provare a eseguire la Federazione di Active Directory locale con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a0450-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="a0450-118">In questo modo, gli utenti possono accedere con le proprie credenziali locali, ma l'autenticazione viene eseguita da Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a0450-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="a0450-119">Per altre informazioni, vedere [autenticazione di Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="a0450-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>

<span data-ttu-id="a0450-120">Per creare un'applicazione che utilizza l'autenticazione integrata di Windows, selezionare il modello "applicazione Intranet" nella creazione guidata progetto MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a0450-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="a0450-121">Questo modello di progetto inserisce le impostazioni seguenti nel file Web. config:</span><span class="sxs-lookup"><span data-stu-id="a0450-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="a0450-122">Sul lato client, l'autenticazione integrata di Windows funziona con qualsiasi browser che supporta lo schema di autenticazione [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , che include la maggior parte dei browser principali.</span><span class="sxs-lookup"><span data-stu-id="a0450-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="a0450-123">Per le applicazioni client .NET, la classe **HttpClient** supporta l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="a0450-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="a0450-124">L'autenticazione di Windows è vulnerabile agli attacchi di richiesta intersito falsificazione (CSRF).</span><span class="sxs-lookup"><span data-stu-id="a0450-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="a0450-125">Vedere [prevenzione degli attacchi di richiesta intersito falsificazione (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="a0450-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
