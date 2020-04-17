---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Autenticazione degli utenti con l'autenticazione di Windows (VB) Documenti Microsoft
author: rick-anderson
description: Informazioni su come utilizzare l'autenticazione di Windows nel contesto di un'applicazione MVC. Si apprenderà come abilitare l'autenticazione di Windows all'interno del co Web dell'applicazione...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 446dcc338f61e1f76478c1085773e7ad089c73f4
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540597"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Autenticazione degli utenti con l'autenticazione di Windows (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Informazioni su come utilizzare l'autenticazione di Windows nel contesto di un'applicazione MVC. Si apprenderà come abilitare l'autenticazione di Windows all'interno del file di configurazione Web dell'applicazione e come configurare l'autenticazione con IIS. Infine, si apprenderà come utilizzare l'attributo [Authorize] per limitare l'accesso alle azioni del controller a determinati utenti o gruppi di Windows.Finally, you learn how to use the [Authorize] attribute to restrict access to controller actions to particular Windows users or groups.

L'obiettivo di questa esercitazione è spiegare come è possibile sfruttare le funzionalità di sicurezza incorporate in Internet Information Services per proteggere con password le visualizzazioni nelle applicazioni MVC. Si apprenderà come consentire alle azioni del controller di essere richiamate solo da utenti o utenti di Windows specifici che sono membri di determinati gruppi di Windows.You learn how to allow controller actions to be invoked only by particular Windows users or users who are members of particular Windows groups.

L'utilizzo dell'autenticazione di Windows ha senso quando si crea un sito Web aziendale interno (un sito Intranet) e si desidera che gli utenti siano in grado di utilizzare i nomi utente e le password standard di Windows quando accedono al sito Web. Se si sta creando un sito Web rivolto verso l'esterno (un sito Web Internet) prendere in considerazione l'utilizzo dell'autenticazione basata su form.

#### <a name="enabling-windows-authentication"></a>Abilitazione dell'autenticazione di WindowsEnabling Windows Authentication

Quando si crea una nuova ASP.NET applicazione MVC, l'autenticazione di Windows non è abilitata per impostazione predefinita. L'autenticazione basata su form è il tipo di autenticazione predefinito abilitato per le applicazioni MVC. È necessario abilitare l'autenticazione di Windows modificando il file di configurazione Web (web.config) dell'applicazione MVC. Trovare &lt;la&gt; sezione di autenticazione e modificarla per utilizzare Windows anziché l'autenticazione basata su form in questo modo:Find the authentication section and modify it to use Windows instead of Forms authentication like this:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Quando si abilita l'autenticazione di Windows, il server Web diventa responsabile dell'autenticazione degli utenti. In genere, esistono due tipi diversi di server Web che si utilizzano durante la creazione e la distribuzione di un'applicazione MVC ASP.NET.

In primo luogo, durante lo sviluppo di un'applicazione MVC, si utilizza il server Web di sviluppo ASP.NET incluso in Visual Studio. Per impostazione predefinita, il server Web di sviluppo ASP.NET esegue tutte le pagine nel contesto dell'account di Windows corrente (qualsiasi account utilizzato per accedere a Windows).

Il server Web di sviluppo ASP.NET supporta anche l'autenticazione NTLM. È possibile abilitare l'autenticazione NTLM facendo clic con il pulsante destro del mouse sul nome del progetto nella finestra Esplora soluzioni e selezionando Proprietà. Successivamente, selezionare la scheda Web e selezionare la casella di controllo NTLM (vedere la figura 1).

**Figura 1 – Abilitazione dell'autenticazione NTLM per il server Web di sviluppo ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Per un'applicazione Web di produzione, è possibile utilizzare IIS come server Web. IIS supporta diversi tipi di autenticazione, tra cui:

- Autenticazione di base: definita come parte del protocollo HTTP 1.0. Invia nomi utente e password in testo non crittografato (codificato Base64) su Internet. - Autenticazione del digest – Invia un hash di una password, anziché la password stessa, attraverso Internet. - Autenticazione integrata di Windows (NTLM): il tipo migliore di autenticazione da utilizzare in ambienti Intranet che utilizzano Windows. - Autenticazione certificato: abilita l'autenticazione utilizzando un certificato lato client.- Certificate Authentication – Enables authentication using a client-side certificate. Il certificato viene mappato a un account utente di Windows.The certificate maps to a Windows user account.

> [!NOTE] 
> 
> Per una panoramica più dettagliata di questi [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx)diversi tipi di autenticazione, vedere .

È possibile utilizzare Gestione Internet Information Services per abilitare un particolare tipo di autenticazione. Tenere presente che tutti i tipi di autenticazione non sono disponibili nel caso di ogni sistema operativo. Inoltre, se si utilizza IIS 7.0 con Windows Vista, sarà necessario abilitare i diversi tipi di autenticazione di Windows prima che vengano visualizzati in Gestione Internet Information Services. Aprire **Pannello di controllo, Programmi, Programmi e funzionalità, Attivare o disattivare le funzionalità di Windows**ed espandere il nodo Internet Information Services (vedere la figura 2).

**Figura 2 – Abilitazione delle funzionalità di Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Utilizzando Internet Information Services, è possibile abilitare o disabilitare diversi tipi di autenticazione. Ad esempio, figura 3 illustra la disabilitazione dell'autenticazione anonima e l'abilitazione dell'autenticazione integrata di Windows (NTLM) quando si utilizza IIS 7.0.

**Figura 3 – Abilitazione dell'autenticazione integrata di Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizzazione di utenti e gruppi di Windows

Dopo aver abilitato l'autenticazione &lt;&gt; di Windows, è possibile usare l'attributo Authorize per controllare l'accesso ai controller o alle azioni del controller. Questo attributo può essere applicato a un intero controller MVC o a una particolare azione del controller.

Ad esempio, il controller Home nel listato 1 espone tre azioni denominate Index(), CompanySecrets() e StephenSecrets(). Chiunque può richiamare l'azione Index(). Tuttavia, solo i membri del gruppo Manager locali di Windows possono richiamare l'azione CompanySecrets(). Infine, solo l'utente di dominio Windows denominato Stephen (nel dominio Redmond) può richiamare l'azione StephenSecrets().

**Listato 1 – Controller-HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> A causa del controllo dell'account utente di Windows (UAC), quando si lavora con Windows Vista o Windows Server 2008, il gruppo Administrators locale si comporterà in modo diverso rispetto ad altri gruppi. L'attributo &lt;Authorize&gt; non riconosce rà correttamente un membro del gruppo Administrators locale a meno che non si modifichino le impostazioni di Controllo dell'account utente del computer.

Esattamente ciò che accade quando si tenta di richiamare un'azione del controller senza essere le autorizzazioni appropriate dipende dal tipo di autenticazione abilitata. Per impostazione predefinita, quando si utilizza il ASP.NET server di sviluppo, è sufficiente ottenere una pagina vuota. La pagina viene servita con uno stato di risposta HTTP **401 non autorizzato.**

Se, d'altra parte, si utilizza IIS con l'autenticazione anonima disabilitata e l'autenticazione di base attivata, quindi si continua a ricevere una finestra di dialogo di accesso ogni volta che si richiede la pagina protetta (vedere Figura 4).

**Figura 4 – Finestra di dialogo di accesso di autenticazione di base**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come utilizzare l'autenticazione di Windows nel contesto di un'applicazione MVC ASP.NET. Si è appreso come abilitare l'autenticazione di Windows all'interno del file di configurazione Web dell'applicazione e come configurare l'autenticazione con IIS. Infine, è stato illustrato &lt;&gt; come utilizzare l'attributo Authorize per limitare l'accesso alle azioni del controller a determinati utenti o gruppi di Windows.Finally, you learned how to use the Authorize attribute to restrict access to controller actions to particular Windows users or groups.

> [!div class="step-by-step"]
> [Successivo](authenticating-users-with-forms-authentication-vb.md)
> [precedente](preventing-javascript-injection-attacks-vb.md)
