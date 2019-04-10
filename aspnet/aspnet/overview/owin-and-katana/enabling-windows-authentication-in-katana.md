---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Abilitazione dell'autenticazione di Windows in Katana | Microsoft Docs
author: MikeWasson
description: "Questo articolo illustra come abilitare l'autenticazione di Windows in Katana. Viene descritto come due scenari: Utilizzo di IIS per host Katana e l'utilizzo di HttpListener di self-hosting Kat..."
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 6d90538ace07402b655b8cd1d9c6e4d5c6dff424
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59411202"
---
# <a name="enabling-windows-authentication-in-katana"></a>Abilitazione dell'autenticazione di Windows in Katana

da [Mike Wasson](https://github.com/MikeWasson)

> Questo articolo illustra come abilitare l'autenticazione di Windows in Katana. Viene descritto come due scenari: Usa IIS per ospitare Katana e uso di HttpListener di self-hosting Katana in un processo personalizzato. Barry Dorrans, David Matson e Chris Ross grazie per aver letto questo articolo.


Katana è l'implementazione Microsoft del [OWIN](http://owin.org/), Open Web Interface for .NET. È possibile leggere un'introduzione a OWIN e Katana [qui](an-overview-of-project-katana.md). L'architettura OWIN ha diversi livelli:

- Organizzatore: Gestisce il processo in cui viene eseguita la pipeline OWIN.
- Server: Apre un socket di rete e ascolta le richieste.
- Middleware: Elabora la richiesta e risposta HTTP.

Katana offre attualmente due server, che supportano entrambi l'autenticazione integrata di Windows:

- **Microsoft.Owin.Host.SystemWeb**. Usa IIS con la pipeline ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Viene utilizzato [System.NET. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Questo server è attualmente l'opzione predefinita quando il Self-hosting Katana.

> [!NOTE]
> Katana attualmente non fornisce il middleware OWIN per l'autenticazione di Windows, perché questa funzionalità è già disponibile nel server.

## <a name="windows-authentication-in-iis"></a>Autenticazione di Windows in IIS

Usa systemweb, è possibile semplicemente abilitare l'autenticazione di Windows in IIS.

Iniziamo creando una nuova applicazione ASP.NET, usando il modello di progetto "Applicazione Web ASP.NET vuota".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Successivamente, aggiungere i pacchetti NuGet. Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Ora aggiungere una classe denominata `Startup` con il codice seguente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Questo è tutto che è necessario creare un'applicazione "Hello world" per OWIN, in esecuzione in IIS. ‎Premere F5 per eseguire il debug dell'applicazione. Si dovrebbe vedere "Hello World!" Nella finestra del browser.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Successivamente, si sarà abilitare l'autenticazione di Windows in IIS Express. Dal **View** dal menu **proprietà**. Fare clic sul nome del progetto in Esplora soluzioni per visualizzare le proprietà del progetto.

Nel **delle proprietà** impostare nella finestra **autenticazione anonima** a **disabilitato** e impostare **l'autenticazione di Windows** a  **Abilitato**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Quando si esegue l'applicazione da Visual Studio, IIS Express richiederà le credenziali di Windows dell'utente. È possibile verificarlo utilizzando [Fiddler](http://fiddler2.com/home) o HTTP di un altro strumento di debug. Di seguito è riportato un esempio di risposta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Le intestazioni WWW-Authenticate nella risposta indicano che il server supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocollo, che usa Kerberos o NTLM.

In un secondo momento, quando si distribuisce l'applicazione in un server, seguire [questi passaggi](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) per abilitare l'autenticazione di Windows in IIS su tale server.

## <a name="windows-authentication-in-httplistener"></a>Autenticazione di Windows in HttpListener

Se si usa Microsoft.Owin.Host.HttpListener indipendente su Katana, è possibile abilitare l'autenticazione di Windows direttamente nel **HttpListener** istanza.

In primo luogo, creare una nuova applicazione console. Successivamente, aggiungere i pacchetti NuGet. Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Ora aggiungere una classe denominata `Startup` con il codice seguente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Questa classe implementa lo stesso esempio "Hello world" dalla prima, ma imposta anche l'autenticazione di Windows come schema di autenticazione.

All'interno di `Main` di funzione, avviare la pipeline OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

È possibile inviare una richiesta in Fiddler per confermare che l'applicazione usa l'autenticazione di Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Argomenti correlati

[Panoramica del progetto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Informazioni sull'autenticazione form OWIN in MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
