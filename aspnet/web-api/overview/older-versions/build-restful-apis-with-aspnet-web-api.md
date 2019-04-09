---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: 'Compilare le API RESTful con ASP.NET Web API: ASP.NET 4.x'
author: rick-anderson
description: "Laboratorio pratico: Utilizzare l'API Web in ASP.NET 4.x per compilare una semplice API REST per un'applicazione Gestione contatti."
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3ba7f2d186e6f0837a32f69f964cec19fe625953
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59391481"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="eba3f-103">Compilare le API RESTful con l'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eba3f-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="eba3f-104">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="eba3f-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="eba3f-105">Laboratorio pratico: Utilizzare l'API Web in ASP.NET 4.x per compilare una semplice API REST per un'applicazione Gestione contatti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="eba3f-106">Si verrà inoltre compilato un client per usare l'API.</span><span class="sxs-lookup"><span data-stu-id="eba3f-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="eba3f-107">Negli ultimi anni, è diventato chiaro che HTTP non è sufficiente per mettere a disposizione le pagine HTML.</span><span class="sxs-lookup"><span data-stu-id="eba3f-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="eba3f-108">È anche una potente piattaforma per la compilazione di API Web, usando ad esempio un numero limitato di verbi (GET, POST e così via) e alcuni concetti semplici *URI* e *intestazioni*.</span><span class="sxs-lookup"><span data-stu-id="eba3f-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="eba3f-109">API Web ASP.NET è un set di componenti che semplificano la programmazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="eba3f-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="eba3f-110">Poiché è basato sul runtime di ASP.NET MVC, API Web gestisce automaticamente i dettagli di basso livello trasporto di HTTP.</span><span class="sxs-lookup"><span data-stu-id="eba3f-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="eba3f-111">Allo stesso tempo, API Web naturalmente espone il modello di programmazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="eba3f-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="eba3f-112">In uno degli obiettivi di API Web è infatti *non* sottraggono la realtà di HTTP.</span><span class="sxs-lookup"><span data-stu-id="eba3f-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="eba3f-113">Di conseguenza, API Web è sia flessibile e facile da estendere.</span><span class="sxs-lookup"><span data-stu-id="eba3f-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="eba3f-114">Lo stile architetturale REST ha dimostrato di essere un metodo efficace per utilizzare HTTP - anche se non è sicuramente l'approccio valido solo per HTTP.</span><span class="sxs-lookup"><span data-stu-id="eba3f-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="eba3f-115">Gestione contatti esporrà il RESTful per gli elenchi, aggiunta e rimozione dei contatti, tra gli altri.</span><span class="sxs-lookup"><span data-stu-id="eba3f-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="eba3f-116">Questa esercitazione richiede una conoscenza di base HTTP, REST e si presuppone una conoscenza di base di codice HTML, JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="eba3f-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="eba3f-117">Il sito Web ASP.NET è presente un'area dedicata per il framework API Web ASP.NET al [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="eba3f-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="eba3f-118">Questo sito continuerà a fornire le informazioni più aggiornate, esempi e le notizie relative all'API Web, quindi verificare spesso se si vuole approfondire la conoscenza l'arte della creazione di API Web personalizzate disponibili per qualsiasi framework di dispositivo o lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="eba3f-119">API Web ASP.NET, simile a ASP.NET MVC 4, ha una grande flessibilità in termini che separa il livello di servizio verso i controller di poterli usare alcuni dei framework di inserimento delle dipendenze disponibili abbastanza facile.</span><span class="sxs-lookup"><span data-stu-id="eba3f-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="eba3f-120">È disponibile un buon esempio in MSDN che illustra come usare Ninject inserimento delle dipendenze in un progetto API Web ASP.NET che è possibile scaricarlo dal [qui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="eba3f-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="eba3f-121">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="eba3f-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="eba3f-122">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="eba3f-122">Objectives</span></span>

<span data-ttu-id="eba3f-123">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="eba3f-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="eba3f-124">Implementare un'API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="eba3f-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="eba3f-125">Chiamare l'API da un client HTML</span><span class="sxs-lookup"><span data-stu-id="eba3f-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="eba3f-126">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="eba3f-126">Prerequisites</span></span>

<span data-ttu-id="eba3f-127">Di seguito è necessario per completare questo laboratorio pratico:</span><span class="sxs-lookup"><span data-stu-id="eba3f-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="eba3f-128">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice B](#AppendixB) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="eba3f-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="eba3f-129">Configurazione</span><span class="sxs-lookup"><span data-stu-id="eba3f-129">Setup</span></span>

**<span data-ttu-id="eba3f-130">Installazione di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="eba3f-130">Installing Code Snippets</span></span>**

<span data-ttu-id="eba3f-131">Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eba3f-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="eba3f-132">Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="eba3f-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="eba3f-133">Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: Uso dei frammenti di codice](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="eba3f-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="eba3f-134">Esercizi</span><span class="sxs-lookup"><span data-stu-id="eba3f-134">Exercises</span></span>

<span data-ttu-id="eba3f-135">Questo laboratorio pratico include l'esercizio seguente:</span><span class="sxs-lookup"><span data-stu-id="eba3f-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="eba3f-136">Esercizio 1: Creare un'API di Web Read-Only</span><span class="sxs-lookup"><span data-stu-id="eba3f-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="eba3f-137">Esercizio 2: Creare un'API Web di lettura/scrittura</span><span class="sxs-lookup"><span data-stu-id="eba3f-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="eba3f-138">Esercizio 3: Usare l'API Web da un Client HTML</span><span class="sxs-lookup"><span data-stu-id="eba3f-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="eba3f-139">Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="eba3f-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="eba3f-140">Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="eba3f-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="eba3f-141">Tempo stimato per completare questa esercitazione: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="eba3f-142">Esercizio 1: Creare un'API di Web Read-Only</span><span class="sxs-lookup"><span data-stu-id="eba3f-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="eba3f-143">In questo esercizio, si implementerà i metodi GET di sola lettura per la gestione di contatti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="eba3f-144">Attività 1: creazione del progetto di API</span><span class="sxs-lookup"><span data-stu-id="eba3f-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="eba3f-145">In questa attività si userà i nuovi modelli di progetto web ASP.NET per creare un'applicazione web API Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="eba3f-146">Eseguire **Visual Studio Express 2012 per Web**, a tale scopo, passare alla **avviare** e digitare **Visual Studio Express per Web** quindi premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="eba3f-147">Dal **File** dal menu **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="eba3f-148">Selezionare il **Visual c# | Web** tipo di visualizzazione struttura ad albero di tipo progetto di progetto, quindi selezionare il **applicazione Web ASP.NET MVC 4** tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="eba3f-149">Impostare il progetto **Name** a *ContactManager* e il **Nome soluzione** a *Begin*, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="eba3f-150">![Creazione di un nuovo progetto ASP.NET MVC 4.0 Web Application](build-restful-apis-with-aspnet-web-api/_static/image1.png "creando un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    *<span data-ttu-id="eba3f-151">Creazione di un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="eba3f-151">Creating a new ASP.NET MVC 4.0 Web Application Project</span></span>*
3. <span data-ttu-id="eba3f-152">Nella finestra di tipo del progetto ASP.NET MVC 4, selezionare la **API Web** tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="eba3f-153">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-153">Click **OK**.</span></span>

    <span data-ttu-id="eba3f-154">![Che specifica il tipo di progetto API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "che specifica il tipo di progetto API Web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    *<span data-ttu-id="eba3f-155">Che specifica il tipo di progetto API Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-155">Specifying the Web API project type</span></span>*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="eba3f-156">Attività 2: creazione di controller dell'API Contact Manager</span><span class="sxs-lookup"><span data-stu-id="eba3f-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="eba3f-157">In questa attività si creerà le classi controller in cui risiederà metodi dell'API.</span><span class="sxs-lookup"><span data-stu-id="eba3f-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="eba3f-158">Eliminare il file denominato **ValuesController.cs** entro **controller** cartella dal progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="eba3f-159">Fare doppio clic il **controller** cartella nel progetto e selezionare **Add | Controller** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="eba3f-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="eba3f-160">![Aggiunge un nuovo controller al progetto](build-restful-apis-with-aspnet-web-api/_static/image3.png "aggiunge un nuovo controller al progetto")</span><span class="sxs-lookup"><span data-stu-id="eba3f-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    *<span data-ttu-id="eba3f-161">Aggiunge un nuovo controller al progetto</span><span class="sxs-lookup"><span data-stu-id="eba3f-161">Adding a new controller to the project</span></span>*
3. <span data-ttu-id="eba3f-162">Nel **Aggiungi Controller** finestra di dialogo visualizzata, selezionare **Controller API vuoto** dal menu modello.</span><span class="sxs-lookup"><span data-stu-id="eba3f-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="eba3f-163">Denominare la classe controller **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="eba3f-164">Quindi, fare clic su **Add.**</span><span class="sxs-lookup"><span data-stu-id="eba3f-164">Then, click **Add.**</span></span>

    <span data-ttu-id="eba3f-165">![Usando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "usando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    *<span data-ttu-id="eba3f-166">Usando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-166">Using the Add Controller dialog to create a new Web API controller</span></span>*
4. <span data-ttu-id="eba3f-167">Aggiungere il codice seguente per il **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="eba3f-168">(Code - Snippet *il Lab di API Web - Ex01 - ottenere il metodo API*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="eba3f-169">Premere **F5** per eseguire il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="eba3f-170">Apparirà la home page predefinita per un progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="eba3f-171">![La home page predefinita di un'applicazione API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "la home page predefinita di un'applicazione API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="eba3f-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    *<span data-ttu-id="eba3f-172">La home page predefinita di un'applicazione API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eba3f-172">The default home page of an ASP.NET Web API application</span></span>*
6. <span data-ttu-id="eba3f-173">Nella finestra di Internet Explorer, premere il **F12** tasto per aprire il **strumenti di sviluppo** finestra.</span><span class="sxs-lookup"><span data-stu-id="eba3f-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="eba3f-174">Fare clic sul **Network** scheda e quindi fare clic sul **Avvia acquisizione** pulsante per avviare l'acquisizione del traffico di rete nella finestra.</span><span class="sxs-lookup"><span data-stu-id="eba3f-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="eba3f-175">![Acquisizione di rete aprendo la scheda di rete e avviando](build-restful-apis-with-aspnet-web-api/_static/image6.png "aprendo la scheda di rete e dell'avvio di acquisizione di rete")</span><span class="sxs-lookup"><span data-stu-id="eba3f-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    *<span data-ttu-id="eba3f-176">Aprendo la scheda di rete e dell'avvio di acquisizione di rete</span><span class="sxs-lookup"><span data-stu-id="eba3f-176">Opening the network tab and initiating network capture</span></span>*
7. <span data-ttu-id="eba3f-177">Aggiungere l'URL nella barra degli indirizzi del browser con **/api/contact** e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="eba3f-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="eba3f-178">I dettagli di trasmissione verranno visualizzato nella finestra di acquisizione di rete.</span><span class="sxs-lookup"><span data-stu-id="eba3f-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="eba3f-179">Si noti che è di tipo MIME della risposta **application/json**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="eba3f-180">Ciò dimostra come il formato di output predefinito è JSON.</span><span class="sxs-lookup"><span data-stu-id="eba3f-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="eba3f-181">![Visualizzazione dell'output della richiesta di API Web nella visualizzazione rete](build-restful-apis-with-aspnet-web-api/_static/image7.png "visualizzando l'output della richiesta di API Web nella visualizzazione rete")</span><span class="sxs-lookup"><span data-stu-id="eba3f-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    *<span data-ttu-id="eba3f-182">Visualizzazione dell'output della richiesta di API Web nella visualizzazione rete</span><span class="sxs-lookup"><span data-stu-id="eba3f-182">Viewing the output of the Web API request in the Network view</span></span>*

    > [!NOTE]
    > <span data-ttu-id="eba3f-183">Comportamento predefinito di Internet Explorer 10 sarà a questo punto per chiedere se l'utente desidera salvare o aprire il flusso risultante dalla chiamata all'API Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="eba3f-184">L'output sarà un file di testo contenente il risultato JSON della chiamata di URL API Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="eba3f-185">Non annullare la finestra di dialogo per poter visualizzare del contenuto della risposta tramite una finestra degli strumenti di sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="eba3f-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="eba3f-186">Scegliere il **passare alla visualizzazione dettagliata** pulsante per visualizzare altri dettagli sulla risposta di questa chiamata API.</span><span class="sxs-lookup"><span data-stu-id="eba3f-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="eba3f-187">![Passare alla visualizzazione dettagliata](build-restful-apis-with-aspnet-web-api/_static/image8.png "passa alla visualizzazione dei dettagli")</span><span class="sxs-lookup"><span data-stu-id="eba3f-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    *<span data-ttu-id="eba3f-188">Passare alla visualizzazione dettagliata</span><span class="sxs-lookup"><span data-stu-id="eba3f-188">Switch to Detailed View</span></span>*
9. <span data-ttu-id="eba3f-189">Scegliere il **corpo della risposta** scheda per visualizzare il testo di risposta JSON effettivo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="eba3f-190">![Visualizzare il codice JSON di output testo il monitoraggio di rete](build-restful-apis-with-aspnet-web-api/_static/image9.png "visualizzando il file JSON di output testo il monitoraggio di rete")</span><span class="sxs-lookup"><span data-stu-id="eba3f-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    *<span data-ttu-id="eba3f-191">Visualizzazione del testo di output JSON in network monitor</span><span class="sxs-lookup"><span data-stu-id="eba3f-191">Viewing the JSON output text in the network monitor</span></span>*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="eba3f-192">Attività 3: creazione di modelli di contatto e aumentare il Controller di contatto</span><span class="sxs-lookup"><span data-stu-id="eba3f-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="eba3f-193">In questa attività si creerà le classi controller in cui risiederà metodi dell'API.</span><span class="sxs-lookup"><span data-stu-id="eba3f-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="eba3f-194">Fare doppio clic il **modelli** cartella e selezionare **Add | Classe...**  dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="eba3f-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="eba3f-195">![Aggiunta di un nuovo modello per l'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image10.png "aggiungendo un nuovo modello per l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    *<span data-ttu-id="eba3f-196">Aggiunta di un nuovo modello per l'applicazione web</span><span class="sxs-lookup"><span data-stu-id="eba3f-196">Adding a new model to the web application</span></span>*
2. <span data-ttu-id="eba3f-197">Nel **Aggiungi nuovo elemento** finestra di dialogo Nome nuovo file **Contact.cs** e fare clic su **Add.**</span><span class="sxs-lookup"><span data-stu-id="eba3f-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="eba3f-198">![Creazione del nuovo file di classe di contatto](build-restful-apis-with-aspnet-web-api/_static/image11.png "creazione del nuovo file di classe di contatto")</span><span class="sxs-lookup"><span data-stu-id="eba3f-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    *<span data-ttu-id="eba3f-199">Creazione del nuovo file di classe di contatto</span><span class="sxs-lookup"><span data-stu-id="eba3f-199">Creating the new Contact class file</span></span>*
3. <span data-ttu-id="eba3f-200">Aggiungere il codice evidenziato seguente per il **contatto** classe.</span><span class="sxs-lookup"><span data-stu-id="eba3f-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="eba3f-201">(Code - Snippet *Web API Lab - Ex01 - contatto classe*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="eba3f-202">Nel **ContactController** classe, selezionare la parola **stringa** nella definizione del metodo del **Ottieni** (metodo) e digitare la parola *contatto*.</span><span class="sxs-lookup"><span data-stu-id="eba3f-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="eba3f-203">Al termine della parola digitata un indicatore verrà visualizzata all'inizio della parola **contatto**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="eba3f-204">Sia premuto la **Ctrl** e premere il tasto punto (.) oppure fare clic sull'icona con il mouse per aprire la finestra di dialogo di assistenza nell'editor del codice, per compilare automaticamente il **usando** direttiva per i modelli spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="eba3f-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *<span data-ttu-id="eba3f-206">Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="eba3f-206">Using Intellisense assistance for namespace declarations</span></span>*
5. <span data-ttu-id="eba3f-207">Modificare il codice per il **ottenere** metodo in modo che restituisca una matrice di istanze di un modello contatto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="eba3f-208">(Code - Snippet *Lab di API Web - Ex01 - restituzione di un elenco di contatti*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="eba3f-209">Premere **F5** al debug dell'applicazione web nel browser.</span><span class="sxs-lookup"><span data-stu-id="eba3f-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="eba3f-210">Per visualizzare le modifiche apportate all'output di risposta dell'API, eseguire la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="eba3f-211">Quando si apre il browser, premere **F12** se gli strumenti di sviluppo non sono ancora aperti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="eba3f-212">Scegliere il **rete** scheda.</span><span class="sxs-lookup"><span data-stu-id="eba3f-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="eba3f-213">Premere il **Avvia acquisizione** pulsante.</span><span class="sxs-lookup"><span data-stu-id="eba3f-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="eba3f-214">Aggiungere il suffisso dell'URL **/api/contact** per l'URL nella barra degli indirizzi e premere il **invio** chiave.</span><span class="sxs-lookup"><span data-stu-id="eba3f-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="eba3f-215">Premere il **passare alla visualizzazione dettagliata** pulsante.</span><span class="sxs-lookup"><span data-stu-id="eba3f-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="eba3f-216">Selezionare il **corpo della risposta** scheda. Dovrebbe essere una stringa JSON che rappresenta il form serializzato di una matrice di istanze del contatto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="eba3f-217">![JSON serializzato output di una chiamata al metodo API Web complessa](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializzato output di una chiamata al metodo API Web complessa")</span><span class="sxs-lookup"><span data-stu-id="eba3f-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      *<span data-ttu-id="eba3f-218">Output JSON serializzato di una chiamata al metodo API Web complessa</span><span class="sxs-lookup"><span data-stu-id="eba3f-218">JSON serialized output of a complex Web API method call</span></span>*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="eba3f-219">Attività 4: funzionalità di estrazione a un livello di servizio</span><span class="sxs-lookup"><span data-stu-id="eba3f-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="eba3f-220">Questa attività verrà illustrato come estrarre funzionalità in un livello di servizio per semplificare per gli sviluppatori separare le funzionalità del servizio dal livello dei controller, consentendo la possibilità di riutilizzo dei servizi che effettivamente svolgono il lavoro.</span><span class="sxs-lookup"><span data-stu-id="eba3f-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="eba3f-221">Creare una nuova cartella nella radice della soluzione e denominarlo **Services**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="eba3f-222">A tale scopo, fare doppio clic su **ContactManager** progetto, selezionare **Add** | **nuova cartella**, denominarla *servizi*.</span><span class="sxs-lookup"><span data-stu-id="eba3f-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="eba3f-223">![Cartella servizi di creazione](build-restful-apis-with-aspnet-web-api/_static/image14.png "cartella la creazione di servizi")</span><span class="sxs-lookup"><span data-stu-id="eba3f-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    *<span data-ttu-id="eba3f-224">Creazione della cartella di servizi</span><span class="sxs-lookup"><span data-stu-id="eba3f-224">Creating Services folder</span></span>*
2. <span data-ttu-id="eba3f-225">Fare doppio clic il **Services** cartella e selezionare **Add | Classe...**  dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="eba3f-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="eba3f-226">![Aggiunta di una nuova classe per la cartella Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "aggiunta una nuova classe per la cartella Services")</span><span class="sxs-lookup"><span data-stu-id="eba3f-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    *<span data-ttu-id="eba3f-227">Aggiunta di una nuova classe per la cartella Services</span><span class="sxs-lookup"><span data-stu-id="eba3f-227">Adding a new class to the Services folder</span></span>*
3. <span data-ttu-id="eba3f-228">Quando la **Aggiungi nuovo elemento** viene visualizzata la finestra, assegnare un nome alla nuova classe **ContactRepository** e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="eba3f-229">![Creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatto](build-restful-apis-with-aspnet-web-api/_static/image16.png "creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti")</span><span class="sxs-lookup"><span data-stu-id="eba3f-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    *<span data-ttu-id="eba3f-230">Creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti</span><span class="sxs-lookup"><span data-stu-id="eba3f-230">Creating a class file to contain the code for the Contact Repository service layer</span></span>*
4. <span data-ttu-id="eba3f-231">Aggiungere un'usando la direttiva per il **ContactRepository.cs** file da includere lo spazio dei nomi modelli.</span><span class="sxs-lookup"><span data-stu-id="eba3f-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="eba3f-232">Aggiungere il codice evidenziato seguente per il **ContactRepository.cs** file per implementare il metodo GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="eba3f-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="eba3f-233">(Code - Snippet *Web API Lab - Ex01 - Repository di contatti*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="eba3f-234">Aprire il **ContactController.cs** file se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="eba3f-235">Aggiungere la seguente istruzione using alla sezione della dichiarazione dello spazio dei nomi del file.</span><span class="sxs-lookup"><span data-stu-id="eba3f-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="eba3f-236">Aggiungere il codice evidenziato seguente per il **ContactController.cs** classe per aggiungere un campo privato per rappresentare l'istanza del repository, in modo che il resto della classe membri possono apportare usare dell'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="eba3f-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="eba3f-237">(Code - Snippet *Web API Lab - Ex01 - Contactcontroller*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="eba3f-238">Modifica il **ottenere** metodo in modo che rende utilizzare il servizio di repository di contatti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="eba3f-239">(Code - Snippet *Lab di API Web - Ex01 - restituzione di un elenco dei contatti tramite il repository*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="eba3f-240">Inserire un punto di interruzione il **ContactController**del **ottenere** definizione di metodo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="eba3f-241">![Aggiunta di punti di interruzione al controller di contatto](build-restful-apis-with-aspnet-web-api/_static/image17.png "aggiungere punti di interruzione al controller di contatto")</span><span class="sxs-lookup"><span data-stu-id="eba3f-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   *<span data-ttu-id="eba3f-242">Aggiunta di punti di interruzione al controller di contatto</span><span class="sxs-lookup"><span data-stu-id="eba3f-242">Adding breakpoints to the contact controller</span></span>*
11. <span data-ttu-id="eba3f-243">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="eba3f-244">Quando si apre il browser, premere **F12** per aprire gli strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="eba3f-245">Scegliere il **rete** scheda.</span><span class="sxs-lookup"><span data-stu-id="eba3f-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="eba3f-246">Scegliere il **Avvia acquisizione** pulsante.</span><span class="sxs-lookup"><span data-stu-id="eba3f-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="eba3f-247">Aggiungere l'URL nella barra degli indirizzi con il suffisso **/api/contact** , quindi premere **invio** per caricare il controller API.</span><span class="sxs-lookup"><span data-stu-id="eba3f-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="eba3f-248">Visual Studio 2012 è presente un'interruzione di una volta **ottenere** metodo inizia l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="eba3f-249">![All'interno del metodo Get di rilievo](build-restful-apis-with-aspnet-web-api/_static/image18.png "rilievo all'interno del metodo Get")</span><span class="sxs-lookup"><span data-stu-id="eba3f-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   *<span data-ttu-id="eba3f-250">Interruzione all'interno del metodo Get</span><span class="sxs-lookup"><span data-stu-id="eba3f-250">Breaking within the Get method</span></span>*
17. <span data-ttu-id="eba3f-251">Premere **F5** per continuare.</span><span class="sxs-lookup"><span data-stu-id="eba3f-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="eba3f-252">Tornare a Internet Explorer se non è già in stato attivo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="eba3f-253">Si noti la finestra di acquisizione di rete.</span><span class="sxs-lookup"><span data-stu-id="eba3f-253">Note the network capture window.</span></span>

    <span data-ttu-id="eba3f-254">![Visualizzazione in Internet Explorer che mostra i risultati della chiamata di API Web di rete](build-restful-apis-with-aspnet-web-api/_static/image19.png "visualizzazione in Internet Explorer che mostra i risultati della chiamata di API Web di rete")</span><span class="sxs-lookup"><span data-stu-id="eba3f-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    *<span data-ttu-id="eba3f-255">Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata di API Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-255">Network view in Internet Explorer showing results of the Web API call</span></span>*
19. <span data-ttu-id="eba3f-256">Scegliere il **passare alla visualizzazione dettagliata** pulsante.</span><span class="sxs-lookup"><span data-stu-id="eba3f-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="eba3f-257">Scegliere il **corpo della risposta** scheda. Si noti l'output JSON della chiamata API e come rappresenta due contatti recuperate dal livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="eba3f-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="eba3f-258">![Visualizzazione dell'output JSON dall'API Web nella finestra di strumenti per sviluppatori](build-restful-apis-with-aspnet-web-api/_static/image20.png "visualizzando l'output JSON dall'API Web nella finestra di strumenti di sviluppo")</span><span class="sxs-lookup"><span data-stu-id="eba3f-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    *<span data-ttu-id="eba3f-259">Visualizzazione dell'output JSON dall'API Web nella finestra di strumenti di sviluppo</span><span class="sxs-lookup"><span data-stu-id="eba3f-259">Viewing the JSON output from the Web API in the developer tools window</span></span>*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="eba3f-260">Esercizio 2: Creare un'API Web di lettura/scrittura</span><span class="sxs-lookup"><span data-stu-id="eba3f-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="eba3f-261">In questo esercizio, si implementerà POST e PUT metodi per la gestione contatti per abilitarlo con le funzionalità di modifica dei dati.</span><span class="sxs-lookup"><span data-stu-id="eba3f-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="eba3f-262">Attività 1: aprire il progetto API Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="eba3f-263">In questa attività viene preparata migliorare il progetto API Web creato nell'esercizio 1, in modo che sia possibile accettare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="eba3f-264">Eseguire **Visual Studio Express 2012 per Web**, a tale scopo, passare alla **avviare** e digitare **Visual Studio Express per Web** quindi premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="eba3f-265">Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex02-ReadWriteWebAPI/** cartella.</span><span class="sxs-lookup"><span data-stu-id="eba3f-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="eba3f-266">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="eba3f-267">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="eba3f-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eba3f-268">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eba3f-269">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eba3f-270">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eba3f-271">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eba3f-272">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eba3f-273">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eba3f-274">Aprire il **Services/ContactRepository.cs** file.</span><span class="sxs-lookup"><span data-stu-id="eba3f-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="eba3f-275">Attività 2: aggiunta di funzionalità di salvataggio permanente dei dati per l'implementazione di Repository di contatti</span><span class="sxs-lookup"><span data-stu-id="eba3f-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="eba3f-276">In questa attività, si verrà Estendi classe ContactRepository del progetto API Web creata nell'esercizio 1, in modo che possa salvare in modo permanente e accettare l'input dell'utente e le nuove istanze di contatto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="eba3f-277">La costante seguente per aggiungere il **ContactRepository** classe per rappresentare il nome del nome del server web cache elemento chiave più avanti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="eba3f-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="eba3f-278">Aggiungere un costruttore per la **ContactRepository** contenente il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="eba3f-279">(Code - Snippet *Web API Lab - Ex02 - Repository di contatti costruttore*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="eba3f-280">Modificare il codice per il **GetAllContacts** metodo come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="eba3f-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="eba3f-281">(Code - Snippet *il Lab di API Web - Ex02 - Ottieni tutti i contatti*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="eba3f-282">Questo esempio è a scopo dimostrativo e utilizzerà la cache del server web come mezzo di archiviazione, in modo che i valori verranno messe a disposizione più client contemporaneamente, anziché usare un meccanismo di archiviazione di sessione o una durata di archiviazione richiesta.</span><span class="sxs-lookup"><span data-stu-id="eba3f-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="eba3f-283">Uno è stato possibile usare Entity Framework, l'archiviazione XML o qualsiasi altra varietà al posto di cache del server web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="eba3f-284">Implementare un nuovo metodo denominato **SaveContact** per il **ContactRepository** classe per le operazioni di salvataggio di un contatto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="eba3f-285">Il **SaveContact** metodo deve accettare un unico **contatto** parametro e restituiscono un valore booleano valore indicante l'esito positivo o negativo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="eba3f-286">(Code - Snippet *Web API Lab - Ex02 - l'implementazione del metodo SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="eba3f-287">Esercizio 3: Usare l'API Web da un Client HTML</span><span class="sxs-lookup"><span data-stu-id="eba3f-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="eba3f-288">In questo esercizio si creerà un client HTML per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="eba3f-289">Questo client faciliterà lo scambio di dati con l'API Web con JavaScript e verrà visualizzati i risultati in un web browser utilizzando il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="eba3f-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="eba3f-290">Attività 1: modificare la visualizzazione dell'indice per fornire un'interfaccia utente grafica per la visualizzazione dei contatti</span><span class="sxs-lookup"><span data-stu-id="eba3f-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="eba3f-291">In questa attività si modificherà la visualizzazione dell'indice predefinito dell'applicazione web per supportare il requisito di visualizzazione elenco dei contatti esistenti in un browser HTML.</span><span class="sxs-lookup"><span data-stu-id="eba3f-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="eba3f-292">Aprire **Visual Studio Express 2012 per Web** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="eba3f-293">Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex03-ConsumingWebAPI/** cartella.</span><span class="sxs-lookup"><span data-stu-id="eba3f-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="eba3f-294">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="eba3f-295">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="eba3f-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eba3f-296">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eba3f-297">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eba3f-298">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eba3f-299">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eba3f-300">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eba3f-301">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eba3f-302">Aprire il **index. cshtml** file si trova in **Views/Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="eba3f-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="eba3f-303">Sostituire il codice HTML all'interno dell'elemento div con id **corpo** in modo che risulti simile al codice seguente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="eba3f-304">Aggiungere il codice Javascript seguente nella parte inferiore del file per eseguire la richiesta HTTP all'API Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="eba3f-305">Aprire il **ContactController.cs** file se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="eba3f-306">Inserire un punto di interruzione il **ottenere** metodo per il **ContactController** classe.</span><span class="sxs-lookup"><span data-stu-id="eba3f-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="eba3f-307">![Inserendo un punto di interruzione nel metodo Get del controller API](build-restful-apis-with-aspnet-web-api/_static/image21.png "inserendo un punto di interruzione nel metodo Get del controller API")</span><span class="sxs-lookup"><span data-stu-id="eba3f-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    *<span data-ttu-id="eba3f-308">Inserendo un punto di interruzione nel metodo Get del controller API</span><span class="sxs-lookup"><span data-stu-id="eba3f-308">Placing a breakpoint on the Get method of the API controller</span></span>*
8. <span data-ttu-id="eba3f-309">Premere **F5** per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-309">Press **F5** to run the project.</span></span> <span data-ttu-id="eba3f-310">Il browser caricherà il documento HTML.</span><span class="sxs-lookup"><span data-stu-id="eba3f-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eba3f-311">Assicurarsi che si sta esplorando l'URL radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="eba3f-312">Dopo il caricamento della pagina e il codice JavaScript viene eseguito, verrà raggiunto il punto di interruzione e sospende l'esecuzione del codice nel controller.</span><span class="sxs-lookup"><span data-stu-id="eba3f-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="eba3f-313">![Debug nelle chiamate all'API Web usando Visual Studio Express per il Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debug approda le chiamate all'API Web usando Visual Studio Express per Web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    *<span data-ttu-id="eba3f-314">Debug di una chiamata API Web usando Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-314">Debugging into the Web API call using Visual Studio 2012 Express for Web</span></span>*
10. <span data-ttu-id="eba3f-315">Rimuovere il punto di interruzione e premere **F5** della barra degli strumenti Debug oppure **continua** pulsante per continuare a caricare la visualizzazione nel browser.</span><span class="sxs-lookup"><span data-stu-id="eba3f-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="eba3f-316">Una volta completata la chiamata all'API Web dovrebbe essere i contatti restituiti dall'API Web chiamate visualizzate come elementi di elenco nel browser.</span><span class="sxs-lookup"><span data-stu-id="eba3f-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="eba3f-317">![Risultati della chiamata API visualizzata nel browser come gli elementi dell'elenco](build-restful-apis-with-aspnet-web-api/_static/image23.png "i risultati della chiamata API visualizzata nel browser come elementi di elenco")</span><span class="sxs-lookup"><span data-stu-id="eba3f-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    *<span data-ttu-id="eba3f-318">Risultati della chiamata API visualizzata nel browser come elementi di elenco</span><span class="sxs-lookup"><span data-stu-id="eba3f-318">Results of the API call displayed in the browser as list items</span></span>*
11. <span data-ttu-id="eba3f-319">Terminare il debug.</span><span class="sxs-lookup"><span data-stu-id="eba3f-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="eba3f-320">Attività 2 - modificare la visualizzazione dell'indice per fornire un'interfaccia utente grafica per la creazione di contatti</span><span class="sxs-lookup"><span data-stu-id="eba3f-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="eba3f-321">In questa attività, si continuerà a modificare la visualizzazione dell'indice dell'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="eba3f-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="eba3f-322">Un modulo verrà aggiunto alla pagina HTML che acquisire l'input dell'utente e inviarlo all'API Web per creare un nuovo contatto e verrà creato un nuovo metodo del controller API Web per la raccolta di date dalla GUI.</span><span class="sxs-lookup"><span data-stu-id="eba3f-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="eba3f-323">Aprire il **ContactController.cs** file.</span><span class="sxs-lookup"><span data-stu-id="eba3f-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="eba3f-324">Aggiungere un nuovo metodo alla classe controller denominata **Post** come illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="eba3f-325">(Code - Snippet *Lab API - Ex03 - Post metodo Web*)</span><span class="sxs-lookup"><span data-stu-id="eba3f-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="eba3f-326">Aprire il **index. cshtml** file in Visual Studio se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="eba3f-327">Aggiungere il codice HTML seguente al file subito dopo l'elenco non ordinato che è stato aggiunto nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="eba3f-328">All'interno dell'elemento di script nella parte inferiore del documento, aggiungere il codice evidenziato seguente per gestire gli eventi clic del pulsante, che pubblicherà i dati nell'API Web mediante una chiamata HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="eba3f-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="eba3f-329">Nelle **ContactController.cs**, inserire un punto di interruzione il **Post** (metodo).</span><span class="sxs-lookup"><span data-stu-id="eba3f-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="eba3f-330">Premere **F5** per eseguire l'applicazione nel browser.</span><span class="sxs-lookup"><span data-stu-id="eba3f-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="eba3f-331">Dopo la pagina viene caricata nel browser, digitare un nuovo nome di contatto e Id e fare clic sui **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="eba3f-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="eba3f-332">![Il documento HTML client caricata nel browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "il documento HTML client caricata nel browser")</span><span class="sxs-lookup"><span data-stu-id="eba3f-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    *<span data-ttu-id="eba3f-333">Il documento HTML client caricata nel browser</span><span class="sxs-lookup"><span data-stu-id="eba3f-333">The client HTML document loaded in the browser</span></span>*
9. <span data-ttu-id="eba3f-334">Quando la finestra del debugger si interrompe **Post** metodo, esaminare le proprietà delle **contattare** parametro.</span><span class="sxs-lookup"><span data-stu-id="eba3f-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="eba3f-335">I valori devono corrispondere i dati che immessi nel form.</span><span class="sxs-lookup"><span data-stu-id="eba3f-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="eba3f-336">![L'oggetto contatto viene inviato all'API Web dal client](build-restful-apis-with-aspnet-web-api/_static/image25.png "oggetto Contact The inviato dal client per l'API Web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    *<span data-ttu-id="eba3f-337">L'oggetto contatto viene inviato all'API Web dal client</span><span class="sxs-lookup"><span data-stu-id="eba3f-337">The Contact object being sent to the Web API from the client</span></span>*
10. <span data-ttu-id="eba3f-338">Passaggio attraverso il metodo nel debugger finché il **risposta** variabile è stata creata.</span><span class="sxs-lookup"><span data-stu-id="eba3f-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="eba3f-339">Al momento di ispezione nel **variabili locali** finestra nel debugger, si noterà che tutte le proprietà sono state impostate.</span><span class="sxs-lookup"><span data-stu-id="eba3f-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="eba3f-340">![La risposta dopo la creazione nel debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "la risposta dopo la creazione nel debugger")</span><span class="sxs-lookup"><span data-stu-id="eba3f-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   *<span data-ttu-id="eba3f-341">La risposta dopo la creazione nel debugger</span><span class="sxs-lookup"><span data-stu-id="eba3f-341">The response following creation in the debugger</span></span>*
11. <span data-ttu-id="eba3f-342">Se si preme **F5** oppure fare clic su **continua** nel debugger verrà completata la richiesta.</span><span class="sxs-lookup"><span data-stu-id="eba3f-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="eba3f-343">Dopo il passaggio al browser, il nuovo contatto è stato aggiunto all'elenco di contatti archiviati per il **ContactRepository** implementazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="eba3f-344">![Il browser riflette termine della creazione della nuova istanza di contatto](build-restful-apis-with-aspnet-web-api/_static/image27.png "browser riflette termine della creazione della nuova istanza di contatto")</span><span class="sxs-lookup"><span data-stu-id="eba3f-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   *<span data-ttu-id="eba3f-345">Il browser riflette termine della creazione della nuova istanza di contatto</span><span class="sxs-lookup"><span data-stu-id="eba3f-345">The browser reflects successful creation of the new contact instance</span></span>*

> [!NOTE]
> <span data-ttu-id="eba3f-346">Inoltre, è possibile distribuire questa applicazione di Azure seguenti [appendice c: Pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="eba3f-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="eba3f-347">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="eba3f-347">Summary</span></span>

<span data-ttu-id="eba3f-348">Questa esercitazione ha illustrato le per il nuovo framework API Web ASP.NET e all'implementazione di API Web RESTful che usano il framework.</span><span class="sxs-lookup"><span data-stu-id="eba3f-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="eba3f-349">A questo punto, è possibile creare un nuovo repository che facilita la persistenza dei dati usando un numero qualsiasi di meccanismi e associare tale servizio anziché quello semplice fornito come esempio in questo laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eba3f-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="eba3f-350">API Web supporta una serie di funzionalità aggiuntive, ad esempio l'abilitazione della comunicazione da client non HTML scritte in qualsiasi linguaggio che supporti HTTP e JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="eba3f-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="eba3f-351">La possibilità di ospitare un'API Web di fuori di un'applicazione web tipica è anche possibile, nonché è la possibilità di creare i propri formati di serializzazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="eba3f-352">Il sito Web ASP.NET è presente un'area dedicata per il framework API Web ASP.NET al [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="eba3f-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="eba3f-353">Questo sito continuerà a fornire le informazioni più aggiornate, esempi e le notizie relative all'API Web, quindi verificare spesso se si vuole approfondire la conoscenza l'arte della creazione di API Web personalizzate disponibili per qualsiasi framework di dispositivo o lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="eba3f-354">Appendice a: Uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="eba3f-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="eba3f-355">Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="eba3f-356">Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="eba3f-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="eba3f-357">![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](build-restful-apis-with-aspnet-web-api/_static/image28.png "frammenti di codice con Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="eba3f-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

*<span data-ttu-id="eba3f-358">Uso di frammenti di codice di Visual Studio per inserire codice nel progetto</span><span class="sxs-lookup"><span data-stu-id="eba3f-358">Using Visual Studio code snippets to insert code into your project</span></span>*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="eba3f-359">Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)</span><span class="sxs-lookup"><span data-stu-id="eba3f-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="eba3f-360">Posizionare il cursore in cui si vuole inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="eba3f-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="eba3f-361">Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="eba3f-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="eba3f-362">Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="eba3f-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="eba3f-363">Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="eba3f-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="eba3f-364">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="eba3f-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="eba3f-365">![Iniziare a digitare il nome di frammento](build-restful-apis-with-aspnet-web-api/_static/image29.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="eba3f-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    *<span data-ttu-id="eba3f-366">Iniziare a digitare il nome del frammento di codice</span><span class="sxs-lookup"><span data-stu-id="eba3f-366">Start typing the snippet name</span></span>*

    <span data-ttu-id="eba3f-367">![Premere Tab per selezionare il frammento di codice evidenziata](build-restful-apis-with-aspnet-web-api/_static/image30.png "premere Tab per selezionare il frammento di codice evidenziata")</span><span class="sxs-lookup"><span data-stu-id="eba3f-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    *<span data-ttu-id="eba3f-368">Premere Tab per selezionare il frammento di codice evidenziata</span><span class="sxs-lookup"><span data-stu-id="eba3f-368">Press Tab to select the highlighted snippet</span></span>*

    <span data-ttu-id="eba3f-369">![Il frammento di codice e premere nuovamente Tab espanderà](build-restful-apis-with-aspnet-web-api/_static/image31.png "si espanderà il frammento di codice e premere nuovamente Tab")</span><span class="sxs-lookup"><span data-stu-id="eba3f-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    *<span data-ttu-id="eba3f-370">Il frammento di codice e premere nuovamente Tab espanderà</span><span class="sxs-lookup"><span data-stu-id="eba3f-370">Press Tab again and the snippet will expand</span></span>*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="eba3f-371">Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)</span><span class="sxs-lookup"><span data-stu-id="eba3f-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="eba3f-372">Pulsante destro del mouse in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="eba3f-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="eba3f-373">Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="eba3f-374">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="eba3f-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="eba3f-375">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](build-restful-apis-with-aspnet-web-api/_static/image32.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="eba3f-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    *<span data-ttu-id="eba3f-376">Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice</span><span class="sxs-lookup"><span data-stu-id="eba3f-376">Right-click where you want to insert the code snippet and select Insert Snippet</span></span>*

    <span data-ttu-id="eba3f-377">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](build-restful-apis-with-aspnet-web-api/_static/image33.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")</span><span class="sxs-lookup"><span data-stu-id="eba3f-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    *<span data-ttu-id="eba3f-378">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa</span><span class="sxs-lookup"><span data-stu-id="eba3f-378">Pick the relevant snippet from the list, by clicking on it</span></span>*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="eba3f-379">Appendice b: Installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="eba3f-380">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="eba3f-381">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="eba3f-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="eba3f-382">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="eba3f-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="eba3f-383">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="eba3f-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="eba3f-384">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-384">Click on **Install Now**.</span></span> <span data-ttu-id="eba3f-385">Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="eba3f-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="eba3f-386">Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="eba3f-387">![Installa Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="eba3f-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    *<span data-ttu-id="eba3f-388">Installa Visual Studio Express</span><span class="sxs-lookup"><span data-stu-id="eba3f-388">Install Visual Studio Express</span></span>*
4. <span data-ttu-id="eba3f-389">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="eba3f-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *<span data-ttu-id="eba3f-391">Accettare le condizioni di licenza</span><span class="sxs-lookup"><span data-stu-id="eba3f-391">Accepting the license terms</span></span>*
5. <span data-ttu-id="eba3f-392">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-392">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *<span data-ttu-id="eba3f-394">Stato dell'installazione</span><span class="sxs-lookup"><span data-stu-id="eba3f-394">Installation progress</span></span>*
6. <span data-ttu-id="eba3f-395">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-395">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *<span data-ttu-id="eba3f-397">Installazione completata</span><span class="sxs-lookup"><span data-stu-id="eba3f-397">Installation completed</span></span>*
7. <span data-ttu-id="eba3f-398">Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="eba3f-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="eba3f-399">Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="eba3f-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express per il riquadro Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *<span data-ttu-id="eba3f-401">Visual Studio Express per il riquadro Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-401">VS Express for Web tile</span></span>*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="eba3f-402">Appendice c: Pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="eba3f-403">In questa appendice spiega come creare un nuovo sito web dal portale di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="eba3f-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="eba3f-404">Attività 1: creazione di un nuovo sito Web dal portale di Azure</span><span class="sxs-lookup"><span data-stu-id="eba3f-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="eba3f-405">Andare alla [portale di gestione Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eba3f-406">Con Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico.</span><span class="sxs-lookup"><span data-stu-id="eba3f-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="eba3f-407">È possibile effettuare l'iscrizione [qui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="eba3f-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="eba3f-408">![Accedere al portale di Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "accedere al portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="eba3f-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    *<span data-ttu-id="eba3f-409">Accedere al portale</span><span class="sxs-lookup"><span data-stu-id="eba3f-409">Log on to Portal</span></span>*
2. <span data-ttu-id="eba3f-410">Fare clic su **New** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="eba3f-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="eba3f-411">![Creazione di un nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    *<span data-ttu-id="eba3f-412">Creazione di un nuovo sito Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-412">Creating a new Web Site</span></span>*
3. <span data-ttu-id="eba3f-413">Fare clic su **Compute** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="eba3f-414">Quindi selezionare **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="eba3f-415">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eba3f-416">Azure è l'host di un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="eba3f-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="eba3f-417">L'opzione Creazione rapida consente di distribuire un'applicazione web completata in Azure all'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="eba3f-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="eba3f-418">Non include i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="eba3f-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="eba3f-419">![Creazione di un nuovo sito Web utilizzando Creazione rapida](build-restful-apis-with-aspnet-web-api/_static/image41.png "creando un nuovo sito Web mediante Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="eba3f-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    *<span data-ttu-id="eba3f-420">Creazione di un nuovo sito Web utilizzando Creazione rapida</span><span class="sxs-lookup"><span data-stu-id="eba3f-420">Creating a new Web Site using Quick Create</span></span>*
4. <span data-ttu-id="eba3f-421">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="eba3f-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="eba3f-422">Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="eba3f-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="eba3f-423">Verificare che il nuovo sito Web sia in funzione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="eba3f-424">![Passare al nuovo sito web](build-restful-apis-with-aspnet-web-api/_static/image42.png "passare al nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    *<span data-ttu-id="eba3f-425">Passare al nuovo sito web</span><span class="sxs-lookup"><span data-stu-id="eba3f-425">Browsing to the new web site</span></span>*

    <span data-ttu-id="eba3f-426">![Sito Web in esecuzione](build-restful-apis-with-aspnet-web-api/_static/image43.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    *<span data-ttu-id="eba3f-427">Sito Web in esecuzione</span><span class="sxs-lookup"><span data-stu-id="eba3f-427">Web site running</span></span>*
6. <span data-ttu-id="eba3f-428">Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="eba3f-429">![Aprire le pagine di gestione sito web](build-restful-apis-with-aspnet-web-api/_static/image44.png "aprire le pagine di gestione sito web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    *<span data-ttu-id="eba3f-430">Aprire le pagine di gestione sito Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-430">Opening the Web Site management pages</span></span>*
7. <span data-ttu-id="eba3f-431">Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="eba3f-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eba3f-432">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="eba3f-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="eba3f-433">Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="eba3f-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba3f-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="eba3f-435">![Download del sito web di profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image45.png "scaricando il sito web di profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    *<span data-ttu-id="eba3f-436">Profilo di pubblicazione scaricato il sito Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-436">Downloading the Web Site publish profile</span></span>*
8. <span data-ttu-id="eba3f-437">Scaricare il file di profilo di pubblicazione in una posizione nota.</span><span class="sxs-lookup"><span data-stu-id="eba3f-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="eba3f-438">In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web in Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eba3f-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="eba3f-439">![Salvare il file di profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image46.png "salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    *<span data-ttu-id="eba3f-440">Salvare il file di profilo di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="eba3f-440">Saving the publish profile file</span></span>*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="eba3f-441">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="eba3f-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="eba3f-442">Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server.</span><span class="sxs-lookup"><span data-stu-id="eba3f-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="eba3f-443">Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="eba3f-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="eba3f-444">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="eba3f-445">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Azure all'indirizzo **database Sql** | **server** | **Dashboard del Server**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="eba3f-446">Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="eba3f-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="eba3f-447">Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva.</span><span class="sxs-lookup"><span data-stu-id="eba3f-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="eba3f-448">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="eba3f-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="eba3f-449">![Dashboard di Server di Database SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard di Server di Database SQL")</span><span class="sxs-lookup"><span data-stu-id="eba3f-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    *<span data-ttu-id="eba3f-450">Dashboard di Server di Database SQL</span><span class="sxs-lookup"><span data-stu-id="eba3f-450">SQL Database Server Dashboard</span></span>*
2. <span data-ttu-id="eba3f-451">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="eba3f-452">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="eba3f-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *<span data-ttu-id="eba3f-454">Aggiunta indirizzo IP del Client</span><span class="sxs-lookup"><span data-stu-id="eba3f-454">Adding Client IP Address</span></span>*
3. <span data-ttu-id="eba3f-455">Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="eba3f-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *<span data-ttu-id="eba3f-457">Confermare le modifiche</span><span class="sxs-lookup"><span data-stu-id="eba3f-457">Confirm Changes</span></span>*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="eba3f-458">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="eba3f-459">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="eba3f-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="eba3f-460">Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="eba3f-461">![Pubblicazione dell'applicazione](build-restful-apis-with-aspnet-web-api/_static/image51.png "pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    *<span data-ttu-id="eba3f-462">Pubblicazione del sito web</span><span class="sxs-lookup"><span data-stu-id="eba3f-462">Publishing the web site</span></span>*
2. <span data-ttu-id="eba3f-463">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="eba3f-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="eba3f-464">![L'importazione del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image52.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    *<span data-ttu-id="eba3f-465">Importazione del profilo di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="eba3f-465">Importing publish profile</span></span>*
3. <span data-ttu-id="eba3f-466">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-466">Click **Validate Connection**.</span></span> <span data-ttu-id="eba3f-467">Dopo aver completata la convalida fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eba3f-468">La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="eba3f-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="eba3f-469">![La convalida connessione](build-restful-apis-with-aspnet-web-api/_static/image53.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    *<span data-ttu-id="eba3f-470">Convalida della connessione</span><span class="sxs-lookup"><span data-stu-id="eba3f-470">Validating connection</span></span>*
4. <span data-ttu-id="eba3f-471">Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="eba3f-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="eba3f-472">![Configurazione della distribuzione Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    *<span data-ttu-id="eba3f-473">Configurazione della distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="eba3f-473">Web deploy configuration</span></span>*
5. <span data-ttu-id="eba3f-474">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="eba3f-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="eba3f-475">Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="eba3f-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="eba3f-476">Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="eba3f-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="eba3f-477">Nelle **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="eba3f-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="eba3f-478">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="eba3f-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="eba3f-479">![Configurazione di stringa di connessione di destinazione](build-restful-apis-with-aspnet-web-api/_static/image55.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="eba3f-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     *<span data-ttu-id="eba3f-480">Configurazione di stringa di connessione di destinazione</span><span class="sxs-lookup"><span data-stu-id="eba3f-480">Configuring destination connection string</span></span>*
6. <span data-ttu-id="eba3f-481">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-481">Then click **OK**.</span></span> <span data-ttu-id="eba3f-482">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="eba3f-483">![Creazione del database](build-restful-apis-with-aspnet-web-api/_static/image56.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="eba3f-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    *<span data-ttu-id="eba3f-484">Creazione del database</span><span class="sxs-lookup"><span data-stu-id="eba3f-484">Creating the database</span></span>*
7. <span data-ttu-id="eba3f-485">La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="eba3f-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="eba3f-486">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-486">Then click **Next**.</span></span>

    <span data-ttu-id="eba3f-487">![Stringa di connessione che punta al Database SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="eba3f-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    *<span data-ttu-id="eba3f-488">Stringa di connessione che punta al Database SQL</span><span class="sxs-lookup"><span data-stu-id="eba3f-488">Connection string pointing to SQL Database</span></span>*
8. <span data-ttu-id="eba3f-489">Nel **Preview** pagina, fare clic su **Publish**.</span><span class="sxs-lookup"><span data-stu-id="eba3f-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="eba3f-490">![Pubblicazione dell'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image58.png "pubblicazione dell'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="eba3f-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    *<span data-ttu-id="eba3f-491">Pubblicazione dell'applicazione web</span><span class="sxs-lookup"><span data-stu-id="eba3f-491">Publishing the web application</span></span>*
9. <span data-ttu-id="eba3f-492">Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="eba3f-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="eba3f-493">![Applicazione pubblicata in Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="eba3f-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    *<span data-ttu-id="eba3f-494">Pubblicata dell'applicazione in Azure</span><span class="sxs-lookup"><span data-stu-id="eba3f-494">Application published to Azure</span></span>*
