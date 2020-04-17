---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Note sulla versione per ASP.NET e gli strumenti Web 2013.1 per Visual Studio 2012 Documenti Microsoft
author: rick-anderson
description: In questo documento viene descritto il rilascio di ASP.NET e Web Tools 2013.1 per Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d4aced4e77a150d52358c2d2513ff79e6594a9de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543574"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="43f14-103">Note sulla versione di ASP.NET and Web Tools 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="43f14-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="43f14-104">da parte [di Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="43f14-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="43f14-105">In questo documento viene descritto il rilascio di ASP.NET e Web Tools 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="43f14-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="43f14-106">Sommario</span><span class="sxs-lookup"><span data-stu-id="43f14-106">Contents</span></span>

- [<span data-ttu-id="43f14-107">Note di installazione</span><span class="sxs-lookup"><span data-stu-id="43f14-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="43f14-108">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="43f14-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="43f14-109">Nuove funzionalità in ASP.NET e Web Tools 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="43f14-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="43f14-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="43f14-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="43f14-111">Modelli</span><span class="sxs-lookup"><span data-stu-id="43f14-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="43f14-112">ASP.NET modello MVC 5</span><span class="sxs-lookup"><span data-stu-id="43f14-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="43f14-113">ASP.NET modello Web API 2</span><span class="sxs-lookup"><span data-stu-id="43f14-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="43f14-114">Modelli di elemento</span><span class="sxs-lookup"><span data-stu-id="43f14-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="43f14-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="43f14-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="43f14-116">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="43f14-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="43f14-117">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="43f14-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="43f14-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="43f14-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="43f14-119">Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes</span><span class="sxs-lookup"><span data-stu-id="43f14-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="43f14-120">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="43f14-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="43f14-121">MVC e API Web Scaffolding - ERRORE HTTP 404, non trovato</span><span class="sxs-lookup"><span data-stu-id="43f14-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="43f14-122">Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento con scaffolding</span><span class="sxs-lookup"><span data-stu-id="43f14-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="43f14-123">ASP.NET Rasoio 3</span><span class="sxs-lookup"><span data-stu-id="43f14-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="43f14-124">La visualizzazione del file cshtml con Sfoglia con o F5 causa un errore del server</span><span class="sxs-lookup"><span data-stu-id="43f14-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="43f14-125">Riscrittura URL e Tilde(</span><span class="sxs-lookup"><span data-stu-id="43f14-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="43f14-126">Modelli</span><span class="sxs-lookup"><span data-stu-id="43f14-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="43f14-127">Note di installazione</span><span class="sxs-lookup"><span data-stu-id="43f14-127">Installation Notes</span></span>

<span data-ttu-id="43f14-128">[Installazione](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET e Web Tools 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="43f14-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="43f14-129">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="43f14-129">Software Requirements</span></span>

<span data-ttu-id="43f14-130">È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per il Web.</span><span class="sxs-lookup"><span data-stu-id="43f14-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="43f14-131">Nuove funzionalità in ASP.NET e Web Tools 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="43f14-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="43f14-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="43f14-132">Bootstrap</span></span>

<span data-ttu-id="43f14-133">Quando si esegue lo scaffolding dei controller e delle visualizzazioni MVC 5, il markup per le visualizzazioni utilizza [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="43f14-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="43f14-134">Modelli</span><span class="sxs-lookup"><span data-stu-id="43f14-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="43f14-135">ASP.NET modello MVC 5</span><span class="sxs-lookup"><span data-stu-id="43f14-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="43f14-136">È stato aggiunto un nuovo modello MVC 5.</span><span class="sxs-lookup"><span data-stu-id="43f14-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="43f14-137">Fa riferimento ai pacchetti NuGet MVC 5 più recenti ed è possibile usare lo scaffolding per aggiungere controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="43f14-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="43f14-138">ASP.NET modello Web API 2</span><span class="sxs-lookup"><span data-stu-id="43f14-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="43f14-139">È stato aggiunto un nuovo modello di API Web 2.We added a new Web API 2 template.</span><span class="sxs-lookup"><span data-stu-id="43f14-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="43f14-140">Fa riferimento ai pacchetti NuGet dell'API Web più recenti ed è possibile usare lo scaffolding per aggiungere controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="43f14-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="43f14-141">Modelli di elementi</span><span class="sxs-lookup"><span data-stu-id="43f14-141">Item Templates</span></span>

<span data-ttu-id="43f14-142">Sono stati aggiunti nuovi modelli di elemento per le visualizzazioni MVC 5, le pagine Web (Razor 3) e i controller Web API 2.</span><span class="sxs-lookup"><span data-stu-id="43f14-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="43f14-143">Installano i pacchetti NuGet correlati al progetto durante l'aggiunta di nuovi elementi.</span><span class="sxs-lookup"><span data-stu-id="43f14-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="43f14-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="43f14-144">Entity Framework 6</span></span>

<span data-ttu-id="43f14-145">Quando si esegue lo scaffolding di un controller MVC o API Web utilizzando Entity Framework, viene usato Framework 6.</span><span class="sxs-lookup"><span data-stu-id="43f14-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="43f14-146">Per ulteriori informazioni su Entity Framework, vedere [cronologia delle versioni](https://msdn.com/data/jj574253)di Entity Framework .</span><span class="sxs-lookup"><span data-stu-id="43f14-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="43f14-147">È inoltre possibile scaricare e installare Entity Framework 6 Tools per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="43f14-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="43f14-148">Vedere [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="43f14-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="43f14-149">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="43f14-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="43f14-150">ASP.NET Scaffolding è un framework di generazione di codice per applicazioni Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="43f14-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="43f14-151">Semplifica l'aggiunta di codice boilerplate al progetto che interagisce con un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="43f14-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="43f14-152">Nelle versioni precedenti di Visual Studio, lo scaffolding era limitato aASP.NET progetti MVC.</span><span class="sxs-lookup"><span data-stu-id="43f14-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="43f14-153">Con questo aggiornamento, è ora possibile utilizzare scaffolding per qualsiasi progetto ASP.NET, inclusi Web Form.</span><span class="sxs-lookup"><span data-stu-id="43f14-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="43f14-154">Questo aggiornamento non supporta la generazione di pagine per un progetto Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo dipendenze MVC al progetto.</span><span class="sxs-lookup"><span data-stu-id="43f14-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="43f14-155">Il supporto per la generazione di pagine per Web Form verrà aggiunto in un aggiornamento futuro.</span><span class="sxs-lookup"><span data-stu-id="43f14-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="43f14-156">Quando si usa lo scaffolding, si garantisce che tutte le dipendenze necessarie siano installate nel progetto.</span><span class="sxs-lookup"><span data-stu-id="43f14-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="43f14-157">Ad esempio, se si inizia con un ASP.NET progetto Web Form e quindi si utilizza lo scaffolding per aggiungere un Controller API Web, i pacchetti E i riferimenti NuGet necessari vengono aggiunti automaticamente al progetto.</span><span class="sxs-lookup"><span data-stu-id="43f14-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="43f14-158">Per aggiungere lo scaffolding MVC a un progetto Web Form, aggiungere un **nuovo elemento con scaffolding** e selezionare **Dipendenze MVC 5** nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="43f14-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="43f14-159">Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo.</span><span class="sxs-lookup"><span data-stu-id="43f14-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="43f14-160">Se si seleziona Minimal, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="43f14-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="43f14-161">Se si seleziona l'opzione Completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC.</span><span class="sxs-lookup"><span data-stu-id="43f14-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="43f14-162">Il supporto per lo scaffolding dei controller asincroni usa le nuove funzionalità asincrone di Entity Framework 6.Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="43f14-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="43f14-163">Per ulteriori informazioni ed esercitazioni, vedere [Cenni ASP.NET scaffolding](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43f14-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="43f14-164">Queste esercitazioni illustrano lo scaffolding con Visual Studio 2013, ma sono applicabili anche ai ASP.NET e Web Tools 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="43f14-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="43f14-165">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="43f14-165">Razor Editor</span></span>

<span data-ttu-id="43f14-166">Con questo aggiornamento, Visual Studio 2012 supporta ora Razor 3 strumenti/editing.</span><span class="sxs-lookup"><span data-stu-id="43f14-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="43f14-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="43f14-167">NuGet 2.7</span></span>

<span data-ttu-id="43f14-168">NuGet 2.7 include un ricco set di nuove funzionalità descritte in dettaglio in [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="43f14-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="43f14-169">Questa versione di NuGet elimina la necessità per gli utenti di consentire in modo esplicito a NuGet di ripristinare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="43f14-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="43f14-170">Quando si installa NuGet 2.7, gli utenti acconsentono implicitamente al ripristino automatico dei pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="43f14-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="43f14-171">Gli utenti possono rifiutare esplicitamente il ripristino del pacchetto tramite le impostazioni NuGet in Visual Studio.Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43f14-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="43f14-172">Questa modifica semplifica il funzionamento del ripristino dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="43f14-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="43f14-173">Problemi noti e modifiche di rilievoKnown Issues and Breaking Changes</span><span class="sxs-lookup"><span data-stu-id="43f14-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="43f14-174">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="43f14-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="43f14-175">MVC e API Web Scaffolding - ERRORE HTTP 404, non trovato</span><span class="sxs-lookup"><span data-stu-id="43f14-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="43f14-176">Se si verifica un errore durante l'aggiunta di un elemento con scaffolding a un progetto, è possibile che il progetto venga lasciato in uno stato incoerente.</span><span class="sxs-lookup"><span data-stu-id="43f14-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="43f14-177">Verrà eseguito il rollback di alcune delle modifiche apportate come scaffolding, ma non verrà eseguito il rollback di altre modifiche, ad esempio i pacchetti NuGet installati.</span><span class="sxs-lookup"><span data-stu-id="43f14-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="43f14-178">Se viene eseguito il rollback delle modifiche alla configurazione di routing, gli utenti riceveranno un errore HTTP 404 durante lo spostamento agli elementi con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="43f14-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="43f14-179">Per correggere questo errore per MVC, aggiungere un nuovo elemento con scaffolding e selezionare Dipendenze MVC 5 (minimo o completo).</span><span class="sxs-lookup"><span data-stu-id="43f14-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="43f14-180">Questo processo aggiungerà tutte le modifiche necessarie al progetto.</span><span class="sxs-lookup"><span data-stu-id="43f14-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="43f14-181">Per correggere questo errore per l'API Web:</span><span class="sxs-lookup"><span data-stu-id="43f14-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="43f14-182">Aggiungere la classe WebApiConfig seguente al progetto.</span><span class="sxs-lookup"><span data-stu-id="43f14-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="43f14-183">Configurare WebApiConfig.Register\_nel metodo Application Start in Global.asax come segue:</span><span class="sxs-lookup"><span data-stu-id="43f14-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="43f14-184">Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento con scaffolding</span><span class="sxs-lookup"><span data-stu-id="43f14-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="43f14-185">Se Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di elemento con scaffolding con Entity Framework (ad esempio Web API 2 Controller con azioni, utilizzando Entity Framework), è possibile che Visual Studio Express non sia riuscito a caricare l'immagine nativa di un assembly dipendente da System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="43f14-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="43f14-186">Per risolvere questo problema, configurare Visual Studio Express per l'utilizzo con l'immagine MSIL di System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="43f14-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="43f14-187">Aprire il prompt dei comandi in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="43f14-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="43f14-188">Andare a %ProgramFiles%, Microsoft Visual Studio 11.0, Common7, IDE o %ProgramFiles(x86)% .</span><span class="sxs-lookup"><span data-stu-id="43f14-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="43f14-189">Aprire VWDExpress.exe.config in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="43f14-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="43f14-190">Aggiungere la riga &lt;seguente&gt;/&lt;&gt; sotto l'elemento runtime di configurazione:</span><span class="sxs-lookup"><span data-stu-id="43f14-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="43f14-191">Riavviare Visual Studio Express 2012 per il Web.</span><span class="sxs-lookup"><span data-stu-id="43f14-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="43f14-192">ASP.NET Rasoio 3</span><span class="sxs-lookup"><span data-stu-id="43f14-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="43f14-193">La visualizzazione del file cshtml con Sfoglia con o F5 causa un errore del server</span><span class="sxs-lookup"><span data-stu-id="43f14-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="43f14-194">Quando si crea un progetto MVC 5 in Visual Studio 2012 (o aprire in Visual Studio 2012 un progetto MVC 5 creato in Visual Studio 2013) e si tenta di visualizzare un file cshtml utilizzando Sfoglia con o F5, verrà visualizzato un errore che indica - **Errore del Server nell'applicazione '/'.**</span><span class="sxs-lookup"><span data-stu-id="43f14-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="43f14-195">Il server tenta di navigare`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="43f14-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="43f14-196">Per risolvere questo problema, modificare l'impostazione **Azione di avvio** nel progetto in **Pagina specifica**.</span><span class="sxs-lookup"><span data-stu-id="43f14-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="43f14-197">Non è necessario fornire un valore per la pagina.</span><span class="sxs-lookup"><span data-stu-id="43f14-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="43f14-198">Dopo aver apportato questa modifica, selezionando F5`http://localhost:XXXX`si passa alla radice dell'applicazione ( ).</span><span class="sxs-lookup"><span data-stu-id="43f14-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="43f14-199">Questo comportamento non corrisponde al comportamento per i progetti MVC 5 in Visual Studio 2013, in cui l'impostazione **Pagina corrente** avvia la pagina aperta.</span><span class="sxs-lookup"><span data-stu-id="43f14-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="43f14-200">Riscrittura URL e Tilde(</span><span class="sxs-lookup"><span data-stu-id="43f14-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="43f14-201">Dopo l'ASP.NET'aggiornamento a ASP.NET Razor 3 o ASP.NET MVC 5, la notazione tilde() potrebbe non funzionare più correttamente se si utilizza la riscrittura URL.</span><span class="sxs-lookup"><span data-stu-id="43f14-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="43f14-202">La riscrittura dell'URL influisce sulla notazione tilde(-) negli&gt; &lt;elementi&gt;HTML &lt;quali A/&gt;, &lt;SCRIPT/ , LINK/ e di conseguenza la tilde non è più mappata alla directory radice.</span><span class="sxs-lookup"><span data-stu-id="43f14-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="43f14-203">Se, ad esempio, si riscrivono le richieste &lt;di **asp.net/content** a **asp.net**, l'attributo href in A **/** href //content/"/&gt; viene risolto in **/content/content/** anziché .</span><span class="sxs-lookup"><span data-stu-id="43f14-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="43f14-204">Per eliminare questa modifica, è possibile impostare il contesto **IIS\_WasUrlRewritten** su false in ogni pagina Web o **\_nell'applicazione BeginRequest** in Global.asax.</span><span class="sxs-lookup"><span data-stu-id="43f14-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="43f14-205">Modelli</span><span class="sxs-lookup"><span data-stu-id="43f14-205">Templates</span></span>

<span data-ttu-id="43f14-206">Quando si creano ASP.NET progetti MVC con Visual Studio 2012 in Windows 8.1 o Windows Server 2012 R2, Visual Studio visualizza un messaggio di errore che indica "Configurazione Web [url] per ASP.NET 4.5 non riuscita."</span><span class="sxs-lookup"><span data-stu-id="43f14-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![errore di configurazione](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="43f14-208">Questo errore viene visualizzato perché Visual Studio 2012 non abilita la funzionalità ASP.NET 4.5 quando è installato in tali versioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="43f14-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="43f14-209">Per abilitare ASP.NET 4.5, eseguire la procedura descritta in [Attivare o disattivare le funzionalità](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)di Windows.</span><span class="sxs-lookup"><span data-stu-id="43f14-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![attivare o disattivare le funzionalità di Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="43f14-211">In alternativa, è possibile abilitare ASP.NET 4.5 tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="43f14-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="43f14-212">Aprire il prompt dei comandi in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="43f14-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="43f14-213">Eseguire il comando seguente per abilitare ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="43f14-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
