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
ms.openlocfilehash: c5fe57c4a20e28fa434659a75484e3a4c37195f8
ms.sourcegitcommit: 000cbcd1de66fe8a766f203ef2d6f009e4435f53
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/16/2020
ms.locfileid: "86424790"
---
# <a name="integrated-windows-authentication"></a>Autenticazione integrata di Windows

di [Mike Wasson](https://github.com/MikeWasson)

Autenticazione integrata di Windows consente agli utenti di accedere con le proprie credenziali di Windows, utilizzando Kerberos o NTLM. Il client invia le credenziali nell'intestazione dell'autorizzazione. L'autenticazione di Windows è particolarmente adatta per un ambiente Intranet. Per altre informazioni, vedere [Autenticazione di Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vantaggi | Svantaggi |
| --- | --- |
| Incorporato in IIS. | Sconsigliato per le applicazioni Internet. | 
| Non invia le credenziali utente nella richiesta. | Richiede il supporto Kerberos o NTLM nel client. |
| Se il computer client appartiene al dominio (ad esempio, l'applicazione Intranet), l'utente non deve immettere le credenziali. | Il client deve trovarsi nel dominio Active Directory. |

> [!NOTE]
> Se l'applicazione è ospitata in Azure e si dispone di un dominio di Active Directory locale, provare a eseguire la Federazione di Active Directory locale con Azure Active Directory. In questo modo, gli utenti possono accedere con le proprie credenziali locali, ma l'autenticazione viene eseguita da Azure AD. Per altre informazioni, vedere [autenticazione di Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Per creare un'applicazione che utilizza l'autenticazione integrata di Windows, selezionare il modello "applicazione Intranet" nella creazione guidata progetto MVC 4. Questo modello di progetto inserisce le impostazioni seguenti nel file di Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Sul lato client, l'autenticazione integrata di Windows funziona con qualsiasi browser che supporta lo schema di autenticazione [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , che include la maggior parte dei browser principali. Per le applicazioni client .NET, la classe **HttpClient** supporta l'autenticazione di Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

L'autenticazione di Windows è vulnerabile agli attacchi di richiesta intersito falsificazione (CSRF). Vedere [prevenzione degli attacchi di richiesta intersito falsificazione (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
