---
title: Supporto di General Data Protection Regulation (GDPR) in ASP.NET Core
author: rick-anderson
description: Informazioni su come accedere ai punti di estensione al GDPR in un'app web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: 5f5ed96354b0b71961c122506602e60b95b809fa
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031958"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="ab7a4-103">Supporto dell'Unione europea protezione il regolamento GDPR (General Data) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab7a4-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="ab7a4-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab7a4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab7a4-105">ASP.NET Core offre API e i modelli per soddisfare alcune delle [EU protezione il regolamento GDPR (General Data)](https://www.eugdpr.org/) requisiti:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="ab7a4-106">I modelli di progetto includono punti di estensione e sottoposto a stub il markup che è possibile sostituire con la privacy e i criteri di utilizzo di cookie.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="ab7a4-107">Una funzionalità di consenso cookie consente richiedere (e tenere traccia) il consenso da parte degli utenti per l'archiviazione dei dati personali.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="ab7a4-108">Se un utente non ha fornito il consenso alla raccolta di dati e l'applicazione dispone [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) impostato su `true`, i cookie non essenziali non vengono inviati al browser.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-108">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="ab7a4-109">I cookie possono essere contrassegnati come essenziali.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="ab7a4-110">Essenziali e vengono inviati al browser, anche quando l'utente non ha fornito il consenso e la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-110">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="ab7a4-111">[I cookie TempData e Session](#tempdata) non sono funzionali durante la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-111">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="ab7a4-112">Il [gestire identità](#pd) pagina fornisce un collegamento per scaricare ed eliminare i dati dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="ab7a4-113">Il [app di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) consente testare la maggior parte dei punti di estensione al GDPR e aggiunte le API per i modelli ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="ab7a4-114">Vedere le [Leggimi](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file per testare le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="ab7a4-115">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab7a4-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="ab7a4-116">Supporto per GDPR ASP.NET Core nel codice del modello generato</span><span class="sxs-lookup"><span data-stu-id="ab7a4-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="ab7a4-117">Razor Pages ed MVC i progetti creati con i modelli di progetto includono il supporto al GDPR:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="ab7a4-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) vengono impostate `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in `Startup`.</span></span>
* <span data-ttu-id="ab7a4-119">Il *_CookieConsentPartial.cshtml* [visualizzazione parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="ab7a4-120">Il *Pages/Privacy.cshtml* pagina oppure *Views/Home/Privacy.cshtml* Vista fornisce una pagina di dettagli sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-120">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="ab7a4-121">Il *_CookieConsentPartial.cshtml* file genera un collegamento alla pagina della Privacy.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-121">The *_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="ab7a4-122">Per le app create con singoli account utente, la pagina di gestione vengono forniti collegamenti per scaricare ed eliminare [i dati personali dell'utente](#pd).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-122">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="ab7a4-123">CookiePolicyOptions e UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="ab7a4-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="ab7a4-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) vengono inizializzate `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="ab7a4-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) viene chiamato `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="ab7a4-126">Visualizzazione parziale _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="ab7a4-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="ab7a4-127">Il *_CookieConsentPartial.cshtml* visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="ab7a4-128">Questo parziale:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-128">This partial:</span></span>

* <span data-ttu-id="ab7a4-129">Ottiene lo stato di rilevamento per l'utente.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-129">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="ab7a4-130">Se l'app è configurato per richiedere il consenso, l'utente deve acconsentire prima che sia possibile rilevare i cookie.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-130">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="ab7a4-131">Se è necessario il consenso, il cookie consenso dei pannelli è fissato nella parte superiore della barra di spostamento creata per il *layout. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-131">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *_Layout.cshtml* file.</span></span>
* <span data-ttu-id="ab7a4-132">Fornisce un elemento HTML `<p>` elemento per riepilogare la privacy e cookie utilizzare criteri.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="ab7a4-133">Fornisce un collegamento alla pagina Privacy o alla vista in cui è possibile descrivere in dettaglio informativa sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-133">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="ab7a4-134">Cookie essenziali</span><span class="sxs-lookup"><span data-stu-id="ab7a4-134">Essential cookies</span></span>

<span data-ttu-id="ab7a4-135">Se il consenso non è stato assegnato, vengono inviati solo i cookie contrassegnati essenziali nel browser.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="ab7a4-136">Il codice seguente effettua un cookie essenziali:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="ab7a4-137">Cookie di stato sessione e provider TempData non sono essenziali</span><span class="sxs-lookup"><span data-stu-id="ab7a4-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="ab7a4-138">Il [provider Tempdata](xref:fundamentals/app-state#tempdata) cookie non è essenziale.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="ab7a4-139">Se la verifica è disabilitata, il provider Tempdata non è funzionale.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-139">If tracking is disabled, the Tempdata provider isn't functional.</span></span> <span data-ttu-id="ab7a4-140">Per abilitare il provider Tempdata quando la verifica è disabilitata, contrassegnare il cookie TempData come essenziale `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="ab7a4-141">[Lo stato della sessione](xref:fundamentals/app-state) i cookie non sono essenziali.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="ab7a4-142">Lo stato della sessione non è attiva quando la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-142">Session state isn't functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="ab7a4-143">Dati personali</span><span class="sxs-lookup"><span data-stu-id="ab7a4-143">Personal data</span></span>

<span data-ttu-id="ab7a4-144">Le app ASP.NET Core create con singoli account utente è incluso codice per scaricare ed eliminare i dati personali.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-144">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="ab7a4-145">Selezionare il nome utente e quindi selezionare **i dati personali**:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-145">Select the user name and then select **Personal data**:</span></span>

![Pagina dati personali di gestione](gdpr/_static/pd.png)

<span data-ttu-id="ab7a4-147">Note:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-147">Notes:</span></span>

* <span data-ttu-id="ab7a4-148">Per generare il `Account/Manage` il codice, vedere [Scaffold identità](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="ab7a4-149">Il **eliminare** e **scaricare** collegamenti solo agiscono sui dati di identità predefinito.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-149">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="ab7a4-150">Le app che creano i dati utente personalizzato devono essere esteso per eliminare e scaricare i dati utente personalizzati.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-150">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="ab7a4-151">Per altre informazioni, vedere [Add, scaricare ed elimina i dati utente personalizzati per Identity](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-151">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="ab7a4-152">Salvare i token per l'utente che vengono archiviati nella tabella di database di identità `AspNetUserTokens` vengono eliminati quando l'utente viene eliminato tramite il comportamento di eliminazione a catena a causa dell'errore il [chiave esterna](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-152">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="ab7a4-153">[Autenticazione basata su provider esterni](xref:security/authentication/social/index), ad esempio Facebook e Google, non è disponibile, prima che venga accettato i criteri per i cookie.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-153">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="ab7a4-154">Crittografia dei dati inattivi</span><span class="sxs-lookup"><span data-stu-id="ab7a4-154">Encryption at rest</span></span>

<span data-ttu-id="ab7a4-155">Alcuni database e meccanismi di archiviazione consentono la crittografia dei dati inattivi.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="ab7a4-156">Crittografia dei dati inattivi:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-156">Encryption at rest:</span></span>

* <span data-ttu-id="ab7a4-157">Consente di crittografare automaticamente i dati archiviati.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="ab7a4-158">Esegue la crittografia senza configurazione, la programmazione o eseguire altre operazioni relative al software che accede ai dati.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="ab7a4-159">È l'opzione più semplice e più sicuro.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="ab7a4-160">Consente al database di gestione delle chiavi e crittografia.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-160">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="ab7a4-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-161">For example:</span></span>

* <span data-ttu-id="ab7a4-162">Microsoft SQL e SQL di Azure forniscono [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="ab7a4-163">SQL Azure consente di crittografare il database per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="ab7a4-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="ab7a4-164">[Azure BLOB, file, tabella e coda di archiviazione vengono crittografati per impostazione predefinita](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="ab7a4-165">Per i database che non offrono la crittografia incorporata quando sono inattivi, è possibile usare crittografia dischi per fornire la protezione stessa.</span><span class="sxs-lookup"><span data-stu-id="ab7a4-165">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="ab7a4-166">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-166">For example:</span></span>

* [<span data-ttu-id="ab7a4-167">BitLocker per Windows Server</span><span class="sxs-lookup"><span data-stu-id="ab7a4-167">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="ab7a4-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="ab7a4-168">Linux:</span></span>
  * [<span data-ttu-id="ab7a4-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="ab7a4-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="ab7a4-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="ab7a4-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab7a4-171">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ab7a4-171">Additional resources</span></span>

* [<span data-ttu-id="ab7a4-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="ab7a4-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)