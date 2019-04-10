---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Gli unit test di applicazioni SignalR | Microsoft Docs
author: bradygaster
description: Questo articolo descrive come usare le funzionalità di Testing unità di SignalR 2.0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 1556e8275da446e285c88d1f850d072725de057b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59415674"
---
# <a name="unit-testing-signalr-applications"></a><span data-ttu-id="b067a-103">Unit test di applicazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="b067a-103">Unit Testing SignalR Applications</span></span>

<span data-ttu-id="b067a-104">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b067a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b067a-105">Questo articolo descrive l'utilizzo delle funzionalità Testing unità di SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="b067a-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b067a-106">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="b067a-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b067a-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b067a-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b067a-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b067a-108">.NET 4.5</span></span>
> - <span data-ttu-id="b067a-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="b067a-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b067a-110">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="b067a-110">Questions and comments</span></span>
>
> <span data-ttu-id="b067a-111">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="b067a-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b067a-112">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b067a-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="b067a-113">Unit test di applicazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="b067a-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="b067a-114">È possibile usare la funzionalità di unit test in SignalR 2 creare unit test per l'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b067a-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="b067a-115">SignalR 2 include i [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaccia, che può essere utilizzato per creare un oggetto fittizio per simulare i metodi dell'hub di test.</span><span class="sxs-lookup"><span data-stu-id="b067a-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="b067a-116">In questa sezione si aggiungerà gli unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando [XUnit.net](https://github.com/xunit/xunit) e [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="b067a-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="b067a-117">XUnit.net verrà usato per controllare il test. Moq consentirà di creare un [simulare](http://en.wikipedia.org/wiki/Mock_object) oggetto per il test.</span><span class="sxs-lookup"><span data-stu-id="b067a-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="b067a-118">Se lo si desidera; è possibile utilizzare altri framework di simulazione [NSubstitute](http://nsubstitute.github.io/) è anche una buona scelta.</span><span class="sxs-lookup"><span data-stu-id="b067a-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="b067a-119">Questa esercitazione illustra come configurare l'oggetto fittizio in due modi: In primo luogo, usando un `dynamic` oggetto (introdotta in .NET Framework 4) e i secondi, utilizzando un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="b067a-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="b067a-120">Sommario</span><span class="sxs-lookup"><span data-stu-id="b067a-120">Contents</span></span>

<span data-ttu-id="b067a-121">Questa esercitazione contiene le sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="b067a-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="b067a-122">Unit test con Dynamic</span><span class="sxs-lookup"><span data-stu-id="b067a-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="b067a-123">Unit test per tipo</span><span class="sxs-lookup"><span data-stu-id="b067a-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="b067a-124">Unit test con Dynamic</span><span class="sxs-lookup"><span data-stu-id="b067a-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="b067a-125">In questa sezione si aggiungerà uno unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) usando un oggetto dinamico.</span><span class="sxs-lookup"><span data-stu-id="b067a-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="b067a-126">Installare il [estensione Runner di XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) per Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b067a-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="b067a-127">Completare la [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare l'applicazione completata [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="b067a-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="b067a-128">Se si usa la versione di download dell'applicazione introduttiva, aprire **Console di gestione pacchetti** e fare clic su **ripristinare** per aggiungere il pacchetto di SignalR per il progetto.</span><span class="sxs-lookup"><span data-stu-id="b067a-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Il ripristino dei pacchetti](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="b067a-130">Aggiungere un progetto alla soluzione per lo unit test.</span><span class="sxs-lookup"><span data-stu-id="b067a-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="b067a-131">Fare doppio clic su della soluzione nel **Esplora soluzioni** e selezionare **Add**, **nuovo progetto...** . Sotto il **c#** nodo, seleziona la **Windows** nodo.</span><span class="sxs-lookup"><span data-stu-id="b067a-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="b067a-132">Selezionare **libreria di classi**.</span><span class="sxs-lookup"><span data-stu-id="b067a-132">Select **Class Library**.</span></span> <span data-ttu-id="b067a-133">Denominare il nuovo progetto **TestLibrary** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b067a-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Creare una libreria di Test](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="b067a-135">Aggiungere un riferimento nel progetto libreria di test al progetto SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="b067a-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="b067a-136">Fare doppio clic il **TestLibrary** del progetto e selezionare **Add**, **riferimento...** . Selezionare il **progetti** nodo sotto il **soluzione** nodo e controllare **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="b067a-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="b067a-137">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b067a-137">Click **OK**.</span></span>

    ![Aggiungi riferimento al progetto](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="b067a-139">Aggiungere i pacchetti di SignalR, Moq e XUnit per il **TestLibrary** progetto.</span><span class="sxs-lookup"><span data-stu-id="b067a-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="b067a-140">Nel **Console di gestione pacchetti**, impostare il **progetto predefinito** elenco a discesa per **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="b067a-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="b067a-141">Nella finestra della console, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b067a-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installare i pacchetti](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="b067a-143">Creare il file di test.</span><span class="sxs-lookup"><span data-stu-id="b067a-143">Create the test file.</span></span> <span data-ttu-id="b067a-144">Fare doppio clic il **TestLibrary** del progetto e fare clic su **Aggiungi...** , **Classe**.</span><span class="sxs-lookup"><span data-stu-id="b067a-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="b067a-145">Denominare la nuova classe **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="b067a-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="b067a-146">Sostituire il contenuto di Tests.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b067a-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="b067a-147">Nel codice precedente, viene creato un client di test usando il `Mock` dall'oggetto di [Moq](https://github.com/Moq/moq4) libreria, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (2.1, SignalR assegnare `dynamic` per il tipo parametro). Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con il quale richiamare metodi sul client.</span><span class="sxs-lookup"><span data-stu-id="b067a-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="b067a-148">Il `broadcastMessage` funzione viene quindi definita per il client fittizio in modo che può essere chiamato dal `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="b067a-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="b067a-149">Il motore di test chiama quindi il `Send` metodo del `ChatHub` (classe), che a sua volta chiama il fittizia `broadcastMessage` (funzione).</span><span class="sxs-lookup"><span data-stu-id="b067a-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="b067a-150">Compilare la soluzione premendo **F6**.</span><span class="sxs-lookup"><span data-stu-id="b067a-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="b067a-151">Eseguire lo unit test.</span><span class="sxs-lookup"><span data-stu-id="b067a-151">Run the unit test.</span></span> <span data-ttu-id="b067a-152">In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**.</span><span class="sxs-lookup"><span data-stu-id="b067a-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="b067a-153">Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.</span><span class="sxs-lookup"><span data-stu-id="b067a-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Esplora test](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="b067a-155">Verificare che il test superato selezionando il riquadro inferiore nella finestra Esplora Test.</span><span class="sxs-lookup"><span data-stu-id="b067a-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="b067a-156">Nella finestra verrà visualizzato che il test è stato superato.</span><span class="sxs-lookup"><span data-stu-id="b067a-156">The window will show that the test passed.</span></span>

    ![Test superato](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="b067a-158">Unit test per tipo</span><span class="sxs-lookup"><span data-stu-id="b067a-158">Unit testing by type</span></span>

<span data-ttu-id="b067a-159">In questa sezione si aggiungerà un test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un'interfaccia che contiene il metodo da sottoporre a test.</span><span class="sxs-lookup"><span data-stu-id="b067a-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="b067a-160">Completare i passaggi da 1 a 7 nella [Unit test con Dynamic](#dynamic) esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="b067a-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="b067a-161">Sostituire il contenuto di Tests.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b067a-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="b067a-162">Nel codice precedente, viene creata un'interfaccia che definisce la firma del `broadcastMessage` metodo per il quale il motore dei test verrà creato un client fittizio.</span><span class="sxs-lookup"><span data-stu-id="b067a-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="b067a-163">Un client fittizio viene quindi creato utilizzando il `Mock` oggetto, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (2.1, SignalR assegnare `dynamic` per il parametro type.) Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con il quale richiamare metodi sul client.</span><span class="sxs-lookup"><span data-stu-id="b067a-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="b067a-164">Il test quindi crea un'istanza di `ChatHub`e quindi crea una versione fittizia del `broadcastMessage` metodo, che a sua volta viene richiamata chiamando il `Send` metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b067a-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="b067a-165">Compilare la soluzione premendo **F6**.</span><span class="sxs-lookup"><span data-stu-id="b067a-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="b067a-166">Eseguire lo unit test.</span><span class="sxs-lookup"><span data-stu-id="b067a-166">Run the unit test.</span></span> <span data-ttu-id="b067a-167">In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**.</span><span class="sxs-lookup"><span data-stu-id="b067a-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="b067a-168">Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.</span><span class="sxs-lookup"><span data-stu-id="b067a-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Esplora test](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="b067a-170">Verificare che il test superato selezionando il riquadro inferiore nella finestra Esplora Test.</span><span class="sxs-lookup"><span data-stu-id="b067a-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="b067a-171">Nella finestra verrà visualizzato che il test è stato superato.</span><span class="sxs-lookup"><span data-stu-id="b067a-171">The window will show that the test passed.</span></span>

    ![Test superato](unit-testing-signalr-applications/_static/image8.png)
