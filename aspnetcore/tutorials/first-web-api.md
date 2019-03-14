---
title: "Esercitazione: Creare un'API Web con ASP.NET Core MVC"
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 02/4/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 686397cd25248ce7b37e505c7129a3b56d4ada1b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045778"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="2c2b3-103">Esercitazione: Creare un'API Web con ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="2c2b3-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="2c2b3-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="2c2b3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2c2b3-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="2c2b3-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c2b3-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-107">Create a web API project.</span></span>
> * <span data-ttu-id="2c2b3-108">Aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-108">Add a model class.</span></span>
> * <span data-ttu-id="2c2b3-109">Creare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-109">Create the database context.</span></span>
> * <span data-ttu-id="2c2b3-110">Registrare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-110">Register the database context.</span></span>
> * <span data-ttu-id="2c2b3-111">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-111">Add a controller.</span></span>
> * <span data-ttu-id="2c2b3-112">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="2c2b3-113">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="2c2b3-114">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-114">Specify return values.</span></span>
> * <span data-ttu-id="2c2b3-115">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="2c2b3-116">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="2c2b3-117">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="2c2b3-118">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2c2b3-118">Overview</span></span>

<span data-ttu-id="2c2b3-119">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="2c2b3-120">API</span><span class="sxs-lookup"><span data-stu-id="2c2b3-120">API</span></span> | <span data-ttu-id="2c2b3-121">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2c2b3-121">Description</span></span> | <span data-ttu-id="2c2b3-122">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="2c2b3-122">Request body</span></span> | <span data-ttu-id="2c2b3-123">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="2c2b3-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="2c2b3-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="2c2b3-124">GET /api/todo</span></span> | <span data-ttu-id="2c2b3-125">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-125">Get all to-do items</span></span> | <span data-ttu-id="2c2b3-126">nessuno</span><span class="sxs-lookup"><span data-stu-id="2c2b3-126">None</span></span> | <span data-ttu-id="2c2b3-127">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-127">Array of to-do items</span></span>|
|<span data-ttu-id="2c2b3-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="2c2b3-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="2c2b3-129">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="2c2b3-129">Get an item by ID</span></span> | <span data-ttu-id="2c2b3-130">nessuno</span><span class="sxs-lookup"><span data-stu-id="2c2b3-130">None</span></span> | <span data-ttu-id="2c2b3-131">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-131">To-do item</span></span>|
|<span data-ttu-id="2c2b3-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="2c2b3-132">POST /api/todo</span></span> | <span data-ttu-id="2c2b3-133">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="2c2b3-133">Add a new item</span></span> | <span data-ttu-id="2c2b3-134">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-134">To-do item</span></span> | <span data-ttu-id="2c2b3-135">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-135">To-do item</span></span> |
|<span data-ttu-id="2c2b3-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="2c2b3-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="2c2b3-137">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2c2b3-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="2c2b3-138">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-138">To-do item</span></span> | <span data-ttu-id="2c2b3-139">nessuno</span><span class="sxs-lookup"><span data-stu-id="2c2b3-139">None</span></span> |
|<span data-ttu-id="2c2b3-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2c2b3-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="2c2b3-141">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2c2b3-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="2c2b3-142">nessuno</span><span class="sxs-lookup"><span data-stu-id="2c2b3-142">None</span></span> | <span data-ttu-id="2c2b3-143">nessuno</span><span class="sxs-lookup"><span data-stu-id="2c2b3-143">None</span></span>|

<span data-ttu-id="2c2b3-144">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-144">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra e invia una richiesta e riceve una risposta dall'applicazione, una casella a destra.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="2c2b3-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="2c2b3-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c2b3-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c2b3-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c2b3-151">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2c2b3-152">Selezionare il modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="2c2b3-153">Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="2c2b3-154">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="2c2b3-155">Selezionare il modello **API**, quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="2c2b3-156">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-156">Do **not** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c2b3-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c2b3-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2c2b3-159">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2c2b3-160">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="2c2b3-161">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="2c2b3-162">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="2c2b3-163">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c2b3-164">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c2b3-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2c2b3-165">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-165">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="2c2b3-167">Selezionare **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="2c2b3-169">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="2c2b3-170">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="2c2b3-172">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="2c2b3-172">Test the API</span></span>

<span data-ttu-id="2c2b3-173">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-173">The project template creates a `values` API.</span></span> <span data-ttu-id="2c2b3-174">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c2b3-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c2b3-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2c2b3-176">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="2c2b3-177">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="2c2b3-178">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="2c2b3-179">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c2b3-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c2b3-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2c2b3-181">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="2c2b3-182">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c2b3-183">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c2b3-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2c2b3-184">Selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="2c2b3-185">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="2c2b3-186">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="2c2b3-187">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="2c2b3-188">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="2c2b3-189">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="2c2b3-189">Add a model class</span></span>

<span data-ttu-id="2c2b3-190">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="2c2b3-191">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c2b3-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c2b3-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c2b3-193">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="2c2b3-194">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2c2b3-195">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="2c2b3-196">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2c2b3-197">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="2c2b3-198">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c2b3-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c2b3-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2c2b3-200">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="2c2b3-201">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c2b3-202">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c2b3-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2c2b3-203">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-203">Right-click the project.</span></span> <span data-ttu-id="2c2b3-204">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2c2b3-205">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-205">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="2c2b3-207">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="2c2b3-208">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="2c2b3-209">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="2c2b3-210">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="2c2b3-211">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="2c2b3-212">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="2c2b3-212">Add a database context</span></span>

<span data-ttu-id="2c2b3-213">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="2c2b3-214">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c2b3-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c2b3-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c2b3-216">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2c2b3-217">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2c2b3-218">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c2b3-218">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2c2b3-219">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-219">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="2c2b3-220">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-220">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="2c2b3-221">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="2c2b3-221">Register the database context</span></span>

<span data-ttu-id="2c2b3-222">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-222">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2c2b3-223">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-223">The container provides the service to controllers.</span></span>

<span data-ttu-id="2c2b3-224">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-224">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="2c2b3-225">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-225">The preceding code:</span></span>

* <span data-ttu-id="2c2b3-226">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-226">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="2c2b3-227">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-227">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="2c2b3-228">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-228">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="2c2b3-229">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="2c2b3-229">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c2b3-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c2b3-230">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c2b3-231">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-231">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="2c2b3-232">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-232">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="2c2b3-233">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-233">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="2c2b3-234">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-234">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2c2b3-236">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c2b3-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2c2b3-237">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-237">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="2c2b3-238">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-238">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="2c2b3-239">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-239">The preceding code:</span></span>

* <span data-ttu-id="2c2b3-240">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-240">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="2c2b3-241">Contrassegnare la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-241">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="2c2b3-242">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-242">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="2c2b3-243">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute) (Annotazione con l'attributo ApiController).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-243">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="2c2b3-244">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-244">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="2c2b3-245">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-245">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="2c2b3-246">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-246">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="2c2b3-247">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-247">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="2c2b3-248">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-248">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="2c2b3-249">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-249">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="2c2b3-250">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="2c2b3-250">Add Get methods</span></span>

<span data-ttu-id="2c2b3-251">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-251">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="2c2b3-252">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-252">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="2c2b3-253">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-253">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="2c2b3-254">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-254">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="2c2b3-255">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-255">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="2c2b3-256">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="2c2b3-256">Routing and URL paths</span></span>

<span data-ttu-id="2c2b3-257">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-257">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="2c2b3-258">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-258">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="2c2b3-259">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-259">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="2c2b3-260">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="2c2b3-260">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="2c2b3-261">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="2c2b3-261">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="2c2b3-262">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-262">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="2c2b3-263">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-263">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="2c2b3-264">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-264">This sample doesn't use a template.</span></span> <span data-ttu-id="2c2b3-265">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-265">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="2c2b3-266">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-266">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="2c2b3-267">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-267">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="2c2b3-268">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="2c2b3-268">Return values</span></span>

<span data-ttu-id="2c2b3-269">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-269">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="2c2b3-270">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-270">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="2c2b3-271">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-271">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="2c2b3-272">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-272">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="2c2b3-273">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-273">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="2c2b3-274">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-274">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="2c2b3-275">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-275">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="2c2b3-276">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-276">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="2c2b3-277">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-277">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="2c2b3-278">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="2c2b3-278">Test the GetTodoItems method</span></span>

<span data-ttu-id="2c2b3-279">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-279">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="2c2b3-280">Installare [Postman](https://www.getpostman.com/apps)</span><span class="sxs-lookup"><span data-stu-id="2c2b3-280">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="2c2b3-281">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-281">Start the web app.</span></span>
* <span data-ttu-id="2c2b3-282">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-282">Start Postman.</span></span>
* <span data-ttu-id="2c2b3-283">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="2c2b3-283">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="2c2b3-284">From  **File > Settings** (File > Impostazioni) (scheda \**General* (Generale)), disattivare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-284">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="2c2b3-285">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-285">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="2c2b3-286">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-286">Create a new request.</span></span>
  * <span data-ttu-id="2c2b3-287">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-287">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="2c2b3-288">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-288">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="2c2b3-289">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-289">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="2c2b3-290">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-290">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="2c2b3-291">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-291">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="2c2b3-293">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="2c2b3-293">Add a Create method</span></span>

<span data-ttu-id="2c2b3-294">Aggiungere il metodo `PostTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-294">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="2c2b3-295">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-295">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="2c2b3-296">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-296">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="2c2b3-297">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-297">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="2c2b3-298">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-298">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="2c2b3-299">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-299">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="2c2b3-300">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-300">Adds a `Location` header to the response.</span></span> <span data-ttu-id="2c2b3-301">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-301">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="2c2b3-302">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-302">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="2c2b3-303">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-303">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="2c2b3-304">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-304">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="2c2b3-305">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="2c2b3-305">Test the PostTodoItem method</span></span>

* <span data-ttu-id="2c2b3-306">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-306">Build the project.</span></span>
* <span data-ttu-id="2c2b3-307">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-307">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="2c2b3-308">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-308">Select the **Body** tab.</span></span>
* <span data-ttu-id="2c2b3-309">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-309">Select the **raw** radio button.</span></span>
* <span data-ttu-id="2c2b3-310">Impostare il tipo su **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-310">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="2c2b3-311">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-311">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="2c2b3-312">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-312">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="2c2b3-314">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-314">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="2c2b3-315">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="2c2b3-315">Test the location header URI</span></span>

* <span data-ttu-id="2c2b3-316">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-316">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="2c2b3-317">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="2c2b3-317">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="2c2b3-319">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-319">Set the method to GET.</span></span>
* <span data-ttu-id="2c2b3-320">Incollare l'URI (ad esempio `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="2c2b3-320">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="2c2b3-321">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-321">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="2c2b3-322">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="2c2b3-322">Add a PutTodoItem method</span></span>

<span data-ttu-id="2c2b3-323">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-323">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="2c2b3-324">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-324">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="2c2b3-325">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-325">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="2c2b3-326">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-326">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="2c2b3-327">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-327">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="2c2b3-328">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che ci sia un elemento nel database.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-328">I you get an error calling `PutTodoItem`, call `GET` to ensure there is a an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="2c2b3-329">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="2c2b3-329">Test the PutTodoItem method</span></span>

<span data-ttu-id="2c2b3-330">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-330">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="2c2b3-331">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-331">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="2c2b3-332">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-332">Call GET to insure there is an item in the database before making a PUT call.</span></span>

<span data-ttu-id="2c2b3-333">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="2c2b3-333">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="2c2b3-334">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-334">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="2c2b3-336">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="2c2b3-336">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="2c2b3-337">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-337">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="2c2b3-338">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-338">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="2c2b3-339">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="2c2b3-339">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="2c2b3-340">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-340">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="2c2b3-341">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-341">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="2c2b3-342">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="2c2b3-342">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="2c2b3-343">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-343">Select **Send**</span></span>

<span data-ttu-id="2c2b3-344">L'app di esempio consente di eliminare tutti gli elementi, ma quando viene eliminato l'ultimo elemento, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-344">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="2c2b3-345">Chiamare l'API con jQuery</span><span class="sxs-lookup"><span data-stu-id="2c2b3-345">Call the API with jQuery</span></span>

<span data-ttu-id="2c2b3-346">In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-346">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="2c2b3-347">jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-347">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="2c2b3-348">Configurare l'app in modo che [gestisca i file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [consenta il mapping di file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span><span class="sxs-lookup"><span data-stu-id="2c2b3-348">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="2c2b3-349">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-349">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="2c2b3-350">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-350">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="2c2b3-351">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-351">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="2c2b3-352">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-352">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="2c2b3-353">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-353">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="2c2b3-354">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-354">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="2c2b3-355">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-355">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="2c2b3-356">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-356">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="2c2b3-357">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-357">There are several ways to get jQuery.</span></span> <span data-ttu-id="2c2b3-358">Nel frammento precedente la libreria viene caricata da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-358">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="2c2b3-359">In questo esempio vengono chiamati tutti i metodi CRUD dell'API.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-359">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="2c2b3-360">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-360">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="2c2b3-361">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-361">Get a list of to-do items</span></span>

<span data-ttu-id="2c2b3-362">La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-362">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="2c2b3-363">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-363">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="2c2b3-364">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-364">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="2c2b3-365">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-365">Add a to-do item</span></span>

<span data-ttu-id="2c2b3-366">La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-366">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="2c2b3-367">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-367">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="2c2b3-368">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-368">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="2c2b3-369">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-369">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="2c2b3-370">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-370">Update a to-do item</span></span>

<span data-ttu-id="2c2b3-371">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-371">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="2c2b3-372">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-372">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="2c2b3-373">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="2c2b3-373">Delete a to-do item</span></span>

<span data-ttu-id="2c2b3-374">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-374">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="2c2b3-375">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2c2b3-375">Additional resources</span></span>

<span data-ttu-id="2c2b3-376">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-376">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="2c2b3-377">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2c2b3-377">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="2c2b3-378">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-378">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="2c2b3-379">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2c2b3-379">Next steps</span></span>

<span data-ttu-id="2c2b3-380">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-380">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c2b3-381">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-381">Create a web api project.</span></span>
> * <span data-ttu-id="2c2b3-382">Aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-382">Add a model class.</span></span>
> * <span data-ttu-id="2c2b3-383">Creare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-383">Create the database context.</span></span>
> * <span data-ttu-id="2c2b3-384">Registrare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-384">Register the database context.</span></span>
> * <span data-ttu-id="2c2b3-385">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-385">Add a controller.</span></span>
> * <span data-ttu-id="2c2b3-386">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-386">Add CRUD methods.</span></span>
> * <span data-ttu-id="2c2b3-387">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-387">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="2c2b3-388">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-388">Specify return values.</span></span>
> * <span data-ttu-id="2c2b3-389">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-389">Call the web API with Postman.</span></span>
> * <span data-ttu-id="2c2b3-390">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="2c2b3-390">Call the web api with jQuery.</span></span>

<span data-ttu-id="2c2b3-391">Passare all'esercitazione successiva per apprendere come generare pagine della Guida dell'API:</span><span class="sxs-lookup"><span data-stu-id="2c2b3-391">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>