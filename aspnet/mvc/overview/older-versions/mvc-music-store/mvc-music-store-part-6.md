---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: utilizzo delle annotazioni dei dati per la convalida del modello | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 6 illustra l'uso delle annotazioni dei dati per il modello V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 24d3f028a9a720e5b526518624c9c1c2ce2c37d4
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044948"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="4f7c6-104">Parte 6: Uso delle annotazioni dei dati per la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="4f7c6-104">Part 6: Using Data Annotations for Model Validation</span></span>

<span data-ttu-id="4f7c6-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4f7c6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4f7c6-106">MVC Music Store è un'applicazione di esercitazione che introduce e illustra in modo dettagliato come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4f7c6-107">MVC Music Store è un'implementazione di archivio di esempio Lightweight che vende album di musica online e implementa l'amministrazione del sito di base, l'accesso degli utenti e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="4f7c6-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4f7c6-109">La parte 6 illustra l'uso delle annotazioni dei dati per la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>

<span data-ttu-id="4f7c6-110">È stato riscontrato un problema importante con i moduli di creazione e modifica: non eseguono alcuna convalida.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="4f7c6-111">È possibile eseguire operazioni come lasciare vuoti i campi o digitare lettere nel campo Price e il primo errore che verrà visualizzato è il database.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="4f7c6-112">È possibile aggiungere facilmente la convalida all'applicazione aggiungendo annotazioni dei dati alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="4f7c6-113">Le annotazioni dei dati consentono di descrivere le regole che si desidera applicare alle proprietà del modello. ASP.NET MVC si occuperà di imporre tali regole e visualizzare i messaggi appropriati agli utenti.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="4f7c6-114">Aggiunta della convalida ai moduli degli album</span><span class="sxs-lookup"><span data-stu-id="4f7c6-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="4f7c6-115">Verranno usati gli attributi di annotazione dei dati seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f7c6-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="4f7c6-116">**Obbligatorio** : indica che la proprietà è un campo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f7c6-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="4f7c6-117">**DisplayName** : definisce il testo da usare nei campi del form e nei messaggi di convalida</span><span class="sxs-lookup"><span data-stu-id="4f7c6-117">**DisplayName** – Defines the text to use on form fields and validation messages</span></span>
- <span data-ttu-id="4f7c6-118">**StringLength** : definisce una lunghezza massima per un campo stringa</span><span class="sxs-lookup"><span data-stu-id="4f7c6-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="4f7c6-119">**Intervallo** : fornisce un valore massimo e minimo per un campo numerico</span><span class="sxs-lookup"><span data-stu-id="4f7c6-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="4f7c6-120">**Binding** : elenca i campi da escludere o includere quando si associano valori di parametri o moduli alle proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="4f7c6-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="4f7c6-121">**ScaffoldColumn** : consente di nascondere i campi dai form dell'editor</span><span class="sxs-lookup"><span data-stu-id="4f7c6-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="4f7c6-122">*Nota: per altre informazioni sulla convalida dei modelli con gli attributi di annotazione dei dati, vedere la documentazione di MSDN all'indirizzo*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="4f7c6-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="4f7c6-123">Aprire la classe album e aggiungere le istruzioni *using* seguenti alla parte superiore.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="4f7c6-124">Aggiornare quindi le proprietà per aggiungere gli attributi di visualizzazione e di convalida, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="4f7c6-125">Anche se siamo qui, abbiamo modificato il genere e l'artista in proprietà virtuali.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="4f7c6-126">Questo consente Entity Framework di caricarli in modo lazy se necessario.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="4f7c6-127">Dopo aver aggiunto questi attributi al modello di album, la schermata di creazione e modifica inizia immediatamente a convalidare i campi e a usare i nomi visualizzati scelti, ad esempio l'URL dell'immagine dell'album anziché AlbumArtUrl.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="4f7c6-128">Eseguire l'applicazione e passare a/StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="4f7c6-129">Verranno quindi violate alcune regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="4f7c6-130">Immettere il prezzo 0 e lasciare vuoto il titolo.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="4f7c6-131">Quando si fa clic sul pulsante Create (Crea), verrà visualizzato il form con messaggi di errore di convalida che indicano quali campi non soddisfano le regole di convalida definite.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="4f7c6-132">Test della convalida lato client</span><span class="sxs-lookup"><span data-stu-id="4f7c6-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="4f7c6-133">La convalida lato server è molto importante dal punto di vista dell'applicazione, perché gli utenti possono eludere la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="4f7c6-134">Tuttavia, i form di pagine Web che implementano solo la convalida sul lato server presentano tre problemi significativi.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="4f7c6-135">L'utente deve attendere che il modulo venga pubblicato, convalidato nel server e che la risposta venga inviata al browser.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="4f7c6-136">L'utente non riceve feedback immediato quando corregge un campo in modo che ora passi le regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="4f7c6-137">Le risorse del server vengono sprecate per eseguire la logica di convalida anziché utilizzare il browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="4f7c6-138">Fortunatamente, i modelli di impalcatura MVC 3 ASP.NET hanno una convalida lato client incorporata, che non richiede alcun lavoro aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="4f7c6-139">Digitare una singola lettera nel campo titolo soddisfa i requisiti di convalida, in modo che il messaggio di convalida venga rimosso immediatamente.</span><span class="sxs-lookup"><span data-stu-id="4f7c6-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4f7c6-140">[Precedente](mvc-music-store-part-5.md) 
>  [Avanti](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="4f7c6-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
