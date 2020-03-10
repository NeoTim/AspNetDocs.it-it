---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Uso di SSL nell'API Web | Microsoft Docs
author: MikeWasson
description: Viene illustrato come utilizzare SSL con API Web ASP.NET, incluso l'utilizzo di certificati client SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598637"
---
# <a name="working-with-ssl-in-web-api"></a><span data-ttu-id="8bac1-103">Working with SSL in Web API (in inglese)</span><span class="sxs-lookup"><span data-stu-id="8bac1-103">Working with SSL in Web API</span></span>

<span data-ttu-id="8bac1-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8bac1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8bac1-105">Diversi schemi di autenticazione comuni non sono sicuri su HTTP normale.</span><span class="sxs-lookup"><span data-stu-id="8bac1-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="8bac1-106">In particolare, l'autenticazione di base e l'autenticazione basata su form inviano credenziali non crittografate.</span><span class="sxs-lookup"><span data-stu-id="8bac1-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="8bac1-107">Per garantire la sicurezza, questi schemi di autenticazione *devono* usare SSL.</span><span class="sxs-lookup"><span data-stu-id="8bac1-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="8bac1-108">Inoltre, è possibile usare i certificati client SSL per autenticare i client.</span><span class="sxs-lookup"><span data-stu-id="8bac1-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="8bac1-109">Abilitazione di SSL nel server</span><span class="sxs-lookup"><span data-stu-id="8bac1-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="8bac1-110">Per configurare SSL in IIS 7 o versione successiva:</span><span class="sxs-lookup"><span data-stu-id="8bac1-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="8bac1-111">Creare o ottenere un certificato.</span><span class="sxs-lookup"><span data-stu-id="8bac1-111">Create or get a certificate.</span></span> <span data-ttu-id="8bac1-112">Per il test, è possibile creare un certificato autofirmato.</span><span class="sxs-lookup"><span data-stu-id="8bac1-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="8bac1-113">Aggiungere un'associazione HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8bac1-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="8bac1-114">Per informazioni dettagliate, vedere [come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="8bac1-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="8bac1-115">Per il test locale, è possibile abilitare SSL in IIS Express da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bac1-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="8bac1-116">Nella Finestra Proprietà impostare **SSL abilitato** su **true**.</span><span class="sxs-lookup"><span data-stu-id="8bac1-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="8bac1-117">Prendere nota del valore dell' **URL SSL**. utilizzare questo URL per testare le connessioni HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8bac1-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="8bac1-118">Applicazione di SSL in un controller API Web</span><span class="sxs-lookup"><span data-stu-id="8bac1-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="8bac1-119">Se si dispone di un'associazione HTTPS e HTTP, i client possono comunque utilizzare HTTP per accedere al sito.</span><span class="sxs-lookup"><span data-stu-id="8bac1-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="8bac1-120">È possibile consentire che alcune risorse siano disponibili tramite HTTP, mentre altre risorse richiedono SSL.</span><span class="sxs-lookup"><span data-stu-id="8bac1-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="8bac1-121">In tal caso, usare un filtro azioni per richiedere SSL per le risorse protette.</span><span class="sxs-lookup"><span data-stu-id="8bac1-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="8bac1-122">Il codice seguente illustra un filtro di autenticazione di API Web che verifica la presenza di SSL:</span><span class="sxs-lookup"><span data-stu-id="8bac1-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="8bac1-123">Aggiungere questo filtro a tutte le azioni di API Web che richiedono SSL:</span><span class="sxs-lookup"><span data-stu-id="8bac1-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="8bac1-124">Certificati client SSL</span><span class="sxs-lookup"><span data-stu-id="8bac1-124">SSL Client Certificates</span></span>

<span data-ttu-id="8bac1-125">SSL fornisce l'autenticazione tramite certificati di infrastruttura a chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="8bac1-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="8bac1-126">Il server deve fornire un certificato che autentica il server per il client.</span><span class="sxs-lookup"><span data-stu-id="8bac1-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="8bac1-127">È meno comune per il client fornire un certificato al server, ma questa è un'opzione per l'autenticazione dei client.</span><span class="sxs-lookup"><span data-stu-id="8bac1-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="8bac1-128">Per usare i certificati client con SSL, è necessario un modo per distribuire i certificati firmati agli utenti.</span><span class="sxs-lookup"><span data-stu-id="8bac1-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="8bac1-129">Per molti tipi di applicazioni, questa non sarà un'esperienza utente corretta, ma in alcuni ambienti (ad esempio, Enterprise) può essere fattibile.</span><span class="sxs-lookup"><span data-stu-id="8bac1-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="8bac1-130">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="8bac1-130">Advantages</span></span> | <span data-ttu-id="8bac1-131">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="8bac1-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="8bac1-132">-Le credenziali del certificato sono più avanzate di nome utente/password.</span><span class="sxs-lookup"><span data-stu-id="8bac1-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="8bac1-133">-SSL fornisce un canale sicuro completo, con autenticazione, integrità dei messaggi e crittografia dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="8bac1-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="8bac1-134">-È necessario ottenere e gestire i certificati PKI.</span><span class="sxs-lookup"><span data-stu-id="8bac1-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="8bac1-135">-La piattaforma client deve supportare i certificati client SSL.</span><span class="sxs-lookup"><span data-stu-id="8bac1-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="8bac1-136">Per configurare IIS in modo che accetti i certificati client, aprire Gestione IIS e seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="8bac1-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="8bac1-137">Fare clic sul nodo sito nella visualizzazione albero.</span><span class="sxs-lookup"><span data-stu-id="8bac1-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="8bac1-138">Fare doppio clic sulla funzionalità **Impostazioni SSL** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="8bac1-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="8bac1-139">In **certificati client**selezionare una di queste opzioni:</span><span class="sxs-lookup"><span data-stu-id="8bac1-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="8bac1-140">**Accept**: IIS accetterà un certificato dal client, ma non ne richieda uno.</span><span class="sxs-lookup"><span data-stu-id="8bac1-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="8bac1-141">**Richiedi**: richiedere un certificato client.</span><span class="sxs-lookup"><span data-stu-id="8bac1-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="8bac1-142">(Per abilitare questa opzione, è necessario selezionare anche "Richiedi SSL")</span><span class="sxs-lookup"><span data-stu-id="8bac1-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="8bac1-143">È anche possibile impostare queste opzioni nel file ApplicationHost. config:</span><span class="sxs-lookup"><span data-stu-id="8bac1-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="8bac1-144">Il flag **SslNegotiateCert** indica che IIS accetterà un certificato dal client, ma non ne richiede uno (equivalente all'opzione "Accept" in Gestione IIS).</span><span class="sxs-lookup"><span data-stu-id="8bac1-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="8bac1-145">Per richiedere un certificato, impostare il flag **SslRequireCert** .</span><span class="sxs-lookup"><span data-stu-id="8bac1-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="8bac1-146">Per il test, è anche possibile impostare queste opzioni in IIS Express nel file ApplicationHost locale. File di configurazione, situato in "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="8bac1-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="8bac1-147">Creazione di un certificato client per il test</span><span class="sxs-lookup"><span data-stu-id="8bac1-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="8bac1-148">A scopo di test, è possibile utilizzare [Makecert. exe](/windows/desktop/SecCrypto/makecert) per creare un certificato client.</span><span class="sxs-lookup"><span data-stu-id="8bac1-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="8bac1-149">Prima di tutto, creare un'autorità radice di test:</span><span class="sxs-lookup"><span data-stu-id="8bac1-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="8bac1-150">MakeCert chiederà di immettere una password per la chiave privata.</span><span class="sxs-lookup"><span data-stu-id="8bac1-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="8bac1-151">Aggiungere quindi il certificato all'archivio "autorità di certificazione radice attendibili" del server di test, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="8bac1-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="8bac1-152">Aprire MMC.</span><span class="sxs-lookup"><span data-stu-id="8bac1-152">Open MMC.</span></span>
2. <span data-ttu-id="8bac1-153">In **file**selezionare **Aggiungi/Rimuovi snap-in**.</span><span class="sxs-lookup"><span data-stu-id="8bac1-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="8bac1-154">Selezionare **account computer**.</span><span class="sxs-lookup"><span data-stu-id="8bac1-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="8bac1-155">Selezionare **computer locale** e completare la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="8bac1-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="8bac1-156">Nel riquadro di spostamento espandere il nodo "autorità di certificazione radice attendibili".</span><span class="sxs-lookup"><span data-stu-id="8bac1-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="8bac1-157">Scegliere **tutte le attività**dal menu **azione** , quindi fare clic su **Importa** per avviare l'importazione guidata certificati.</span><span class="sxs-lookup"><span data-stu-id="8bac1-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="8bac1-158">Individuare il file del certificato, TempCA. cer.</span><span class="sxs-lookup"><span data-stu-id="8bac1-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="8bac1-159">Fare clic su **Apri**, quindi su **Avanti** e completare la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="8bac1-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="8bac1-160">(Verrà richiesto di immettere nuovamente la password).</span><span class="sxs-lookup"><span data-stu-id="8bac1-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="8bac1-161">A questo punto creare un certificato client firmato dal primo certificato:</span><span class="sxs-lookup"><span data-stu-id="8bac1-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="8bac1-162">Uso dei certificati client nell'API Web</span><span class="sxs-lookup"><span data-stu-id="8bac1-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="8bac1-163">Sul lato server, è possibile ottenere il certificato client chiamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) nel messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="8bac1-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="8bac1-164">Il metodo restituisce null se non è presente alcun certificato client.</span><span class="sxs-lookup"><span data-stu-id="8bac1-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="8bac1-165">In caso contrario, restituisce un'istanza di **X509Certificate2** .</span><span class="sxs-lookup"><span data-stu-id="8bac1-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="8bac1-166">Utilizzare questo oggetto per ottenere informazioni dal certificato, ad esempio l'autorità emittente e il soggetto.</span><span class="sxs-lookup"><span data-stu-id="8bac1-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="8bac1-167">È quindi possibile usare queste informazioni per l'autenticazione e/o l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="8bac1-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
