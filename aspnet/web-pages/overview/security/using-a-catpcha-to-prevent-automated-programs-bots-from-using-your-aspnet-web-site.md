---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Utilizzo di un CAPTCHA per impedire ai bot di utilizzare il sito ASP.NET Web Razor) Documenti Microsoft
author: rick-anderson
description: In questo articolo viene illustrato come utilizzare ReCaptcha (una misura di sicurezza) per impedire ai programmi automatizzati (bot) di eseguire attività in un ASP.NET Pagine Web (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 65f414ae3fed5e2fa28b1e57f5327c6411a43d55
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543756"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Utilizzo di un CAPTCHA per impedire ai bot di utilizzare il sito ASP.NET Web Razor)

da parte [di Microsoft](https://github.com/microsoft)

> In questo articolo viene illustrato come utilizzare ReCaptcha (una misura di sicurezza) per impedire ai programmi automatizzati (bot) di eseguire attività in un sito Web di pagine Web ASP.NET.
> 
> **Contenuto dell'esercitazione:** 
> 
> - Come aggiungere un test CAPTCHA al tuo sito.
> 
> Queste sono le caratteristiche ASP.NET introdotte nell'articolo:
> 
> - L'aiutante. `ReCaptcha`
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a ASP.NET pagine Web 1.0 e pagine Web 2.

## <a name="about-captchas"></a>Informazioni sui CAPTCHA

Ogni volta che consenti alle persone di registrarsi nel tuo sito, o anche solo inserire un nome e un URL (come per un commento sul blog), potresti ottenere una marea di nomi falsi. Questi sono spesso lasciati da programmi automatizzati (bot) che cercano di lasciare gli URL in ogni sito web che possono trovare. (Una motivazione comune è quella di pubblicare gli URL dei prodotti in vendita.)

Puoi assicurarti che un utente sia una persona reale e non un programma per computer utilizzando un *CAPTCHA* per convalidare gli utenti quando si registrano o immettono in altro modo il proprio nome e il proprio sito. CAPTCHA è l'acronimo di Test di Turing pubblico completamente automatizzato per raccontare computer e esseri umani a parte. Un CAPTCHA è un test *challenge-response* in cui all'utente viene chiesto di fare qualcosa che è facile da fare per una persona, ma difficile da fare per un programma automatizzato. Il tipo più comune di CAPTCHA è quello in cui si vedono alcune lettere distorte e viene chiesto di digitarle. (La distorsione dovrebbe rendere difficile per i bot decifrare le lettere.)

## <a name="adding-a-recaptcha-test"></a>Aggiunta di un test DiCaptcha

Nelle pagine ASP.NET è `ReCaptcha` possibile utilizzare l'helper per eseguire il rendering di un[http://recaptcha.net](http://recaptcha.net)test CAPTCHA basato sul servizio ReCaptcha ( ). L'helper `ReCaptcha` visualizza un'immagine di due parole distorte che gli utenti devono immettere correttamente prima che la pagina venga convalidata. La risposta dell'utente viene convalidata dal servizio ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registra il tuo[http://recaptcha.net](http://recaptcha.net)sito web allReCaptcha.Net ( ). Una volta completata la registrazione, si otterranno una chiave pubblica e una chiave privata.
2. Aggiungere la ASP.NET raccolta di web helper al sito Web come descritto in [Installazione degli helper in un sito](https://go.microsoft.com/fwlink/?LinkId=252372)di pagine Web ASP.NET , se non è già stato fatto.
3. Se non si dispone già di un * \_file AppStart.cshtml,* nella cartella radice di un sito Web creare un file denominato * \_AppStart.cshtml*.
4. Aggiungere le `Recaptcha` seguenti impostazioni helper nel file * \_AppStart.cshtml:* 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Impostare `PublicKey` `PrivateKey` le proprietà e utilizzando le proprie chiavi pubbliche e private.
6. Salvare il * \_file AppStart.cshtml* e chiuderlo.
7. Nella cartella principale di un sito Web creare una nuova pagina *denominata Recaptcha.cshtml*.
8. Sostituire il contenuto esistente con quanto segue: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Eseguire la pagina *Recaptcha.cshtml* in un browser. Se `PrivateKey` il valore è valido, nella pagina vengono visualizzati il controllo ReCaptcha e un pulsante. Se le chiavi non fossero impostate globalmente in * \_AppStart.html*, nella pagina verrà visualizzato un errore. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Immettere le parole per il test. Se superi il test ReCaptcha, viene visualizzato un messaggio in tal senso. In caso contrario viene visualizzato un messaggio di errore e il controllo ReCaptcha viene visualizzato nuovamente.

> [!NOTE]
> Se il computer si trova in un dominio che `defaultproxy` utilizza un server proxy, potrebbe essere necessario configurare l'elemento del file *Web.config.* Nell'esempio seguente viene illustrato un `defaultproxy` file *Web.config* con l'elemento configurato per consentire il funzionamento del servizio ReCaptcha.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Personalizzazione del comportamento a livello di sito per ASP.NET siti di pagine Web](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sito di ReCaptcha](https://www.google.com/recaptcha)
