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
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="438c2-103">Utilizzo di un CAPTCHA per impedire ai bot di utilizzare il sito ASP.NET Web Razor)</span><span class="sxs-lookup"><span data-stu-id="438c2-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="438c2-104">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="438c2-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="438c2-105">In questo articolo viene illustrato come utilizzare ReCaptcha (una misura di sicurezza) per impedire ai programmi automatizzati (bot) di eseguire attività in un sito Web di pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="438c2-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="438c2-106">**Contenuto dell'esercitazione:**</span><span class="sxs-lookup"><span data-stu-id="438c2-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="438c2-107">Come aggiungere un test CAPTCHA al tuo sito.</span><span class="sxs-lookup"><span data-stu-id="438c2-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="438c2-108">Queste sono le caratteristiche ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="438c2-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="438c2-109">L'aiutante. `ReCaptcha`</span><span class="sxs-lookup"><span data-stu-id="438c2-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="438c2-110">Le informazioni contenute in questo articolo si applicano a ASP.NET pagine Web 1.0 e pagine Web 2.</span><span class="sxs-lookup"><span data-stu-id="438c2-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="438c2-111">Informazioni sui CAPTCHA</span><span class="sxs-lookup"><span data-stu-id="438c2-111">About CAPTCHAs</span></span>

<span data-ttu-id="438c2-112">Ogni volta che consenti alle persone di registrarsi nel tuo sito, o anche solo inserire un nome e un URL (come per un commento sul blog), potresti ottenere una marea di nomi falsi.</span><span class="sxs-lookup"><span data-stu-id="438c2-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="438c2-113">Questi sono spesso lasciati da programmi automatizzati (bot) che cercano di lasciare gli URL in ogni sito web che possono trovare.</span><span class="sxs-lookup"><span data-stu-id="438c2-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="438c2-114">(Una motivazione comune è quella di pubblicare gli URL dei prodotti in vendita.)</span><span class="sxs-lookup"><span data-stu-id="438c2-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="438c2-115">Puoi assicurarti che un utente sia una persona reale e non un programma per computer utilizzando un *CAPTCHA* per convalidare gli utenti quando si registrano o immettono in altro modo il proprio nome e il proprio sito.</span><span class="sxs-lookup"><span data-stu-id="438c2-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="438c2-116">CAPTCHA è l'acronimo di Test di Turing pubblico completamente automatizzato per raccontare computer e esseri umani a parte.</span><span class="sxs-lookup"><span data-stu-id="438c2-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="438c2-117">Un CAPTCHA è un test *challenge-response* in cui all'utente viene chiesto di fare qualcosa che è facile da fare per una persona, ma difficile da fare per un programma automatizzato.</span><span class="sxs-lookup"><span data-stu-id="438c2-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="438c2-118">Il tipo più comune di CAPTCHA è quello in cui si vedono alcune lettere distorte e viene chiesto di digitarle.</span><span class="sxs-lookup"><span data-stu-id="438c2-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="438c2-119">(La distorsione dovrebbe rendere difficile per i bot decifrare le lettere.)</span><span class="sxs-lookup"><span data-stu-id="438c2-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="438c2-120">Aggiunta di un test DiCaptcha</span><span class="sxs-lookup"><span data-stu-id="438c2-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="438c2-121">Nelle pagine ASP.NET è `ReCaptcha` possibile utilizzare l'helper per eseguire il rendering di un[http://recaptcha.net](http://recaptcha.net)test CAPTCHA basato sul servizio ReCaptcha ( ).</span><span class="sxs-lookup"><span data-stu-id="438c2-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="438c2-122">L'helper `ReCaptcha` visualizza un'immagine di due parole distorte che gli utenti devono immettere correttamente prima che la pagina venga convalidata.</span><span class="sxs-lookup"><span data-stu-id="438c2-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="438c2-123">La risposta dell'utente viene convalidata dal servizio ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="438c2-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="438c2-124">Registra il tuo[http://recaptcha.net](http://recaptcha.net)sito web allReCaptcha.Net ( ).</span><span class="sxs-lookup"><span data-stu-id="438c2-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="438c2-125">Una volta completata la registrazione, si otterranno una chiave pubblica e una chiave privata.</span><span class="sxs-lookup"><span data-stu-id="438c2-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="438c2-126">Aggiungere la ASP.NET raccolta di web helper al sito Web come descritto in [Installazione degli helper in un sito](https://go.microsoft.com/fwlink/?LinkId=252372)di pagine Web ASP.NET , se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="438c2-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="438c2-127">Se non si dispone già di un \* \_file AppStart.cshtml,\* nella cartella radice di un sito Web creare un file denominato \* \_AppStart.cshtml\*.</span><span class="sxs-lookup"><span data-stu-id="438c2-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="438c2-128">Aggiungere le `Recaptcha` seguenti impostazioni helper nel file \* \_AppStart.cshtml:\*</span><span class="sxs-lookup"><span data-stu-id="438c2-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="438c2-129">Impostare `PublicKey` `PrivateKey` le proprietà e utilizzando le proprie chiavi pubbliche e private.</span><span class="sxs-lookup"><span data-stu-id="438c2-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="438c2-130">Salvare il \* \_file AppStart.cshtml\* e chiuderlo.</span><span class="sxs-lookup"><span data-stu-id="438c2-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="438c2-131">Nella cartella principale di un sito Web creare una nuova pagina *denominata Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="438c2-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="438c2-132">Sostituire il contenuto esistente con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="438c2-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="438c2-133">Eseguire la pagina *Recaptcha.cshtml* in un browser.</span><span class="sxs-lookup"><span data-stu-id="438c2-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="438c2-134">Se `PrivateKey` il valore è valido, nella pagina vengono visualizzati il controllo ReCaptcha e un pulsante.</span><span class="sxs-lookup"><span data-stu-id="438c2-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="438c2-135">Se le chiavi non fossero impostate globalmente in \* \_AppStart.html\*, nella pagina verrà visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="438c2-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="438c2-136">Immettere le parole per il test.</span><span class="sxs-lookup"><span data-stu-id="438c2-136">Enter the words for the test.</span></span> <span data-ttu-id="438c2-137">Se superi il test ReCaptcha, viene visualizzato un messaggio in tal senso.</span><span class="sxs-lookup"><span data-stu-id="438c2-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="438c2-138">In caso contrario viene visualizzato un messaggio di errore e il controllo ReCaptcha viene visualizzato nuovamente.</span><span class="sxs-lookup"><span data-stu-id="438c2-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="438c2-139">Se il computer si trova in un dominio che `defaultproxy` utilizza un server proxy, potrebbe essere necessario configurare l'elemento del file *Web.config.*</span><span class="sxs-lookup"><span data-stu-id="438c2-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="438c2-140">Nell'esempio seguente viene illustrato un `defaultproxy` file *Web.config* con l'elemento configurato per consentire il funzionamento del servizio ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="438c2-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="438c2-141">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="438c2-141">Additional Resources</span></span>

- [<span data-ttu-id="438c2-142">Personalizzazione del comportamento a livello di sito per ASP.NET siti di pagine Web</span><span class="sxs-lookup"><span data-stu-id="438c2-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="438c2-143">Sito di ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="438c2-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
