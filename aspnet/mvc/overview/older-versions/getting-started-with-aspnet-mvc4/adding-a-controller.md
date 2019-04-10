---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Aggiunta di un Controller | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d1cd01e924dc8e13b22b736ada490a3507e730f5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59405833"
---
# <a name="adding-a-controller"></a>Aggiunta di un controller

da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


MVC è l'acronimo *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni che sono ben strutturata, testabile e facile da gestire. Le applicazioni basate su MVC contengono:

- **M** odelli: Classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.
- **V** iste: File di modello che l'applicazione usa per generare in modo dinamico le risposte HTML.
- **C** ontroller: Classi che gestiscono le richieste in ingresso del browser, recuperano i dati del modello e quindi specificare i modelli di vista che restituiscono una risposta nel browser.

Si verrà che coprono tutti questi concetti in questa serie di esercitazioni e mostrerò come usarle per compilare un'applicazione.

È innanzitutto necessario creare una classe controller. Nella **Esplora soluzioni**, fare doppio clic il *controller* cartella e quindi selezionare **Aggiungi Controller**.

![](adding-a-controller/_static/image1.png)

Assegnare un nome al controller &quot;HelloWorldController&quot;. Lasciare il modello predefinito come **controller MVC vuoto** e fare clic su **Add**.

![Aggiungi controller](adding-a-controller/_static/image2.png)

Si noti che nel **Esplora soluzioni** che un nuovo file creato denominato *HelloWorldController.cs*. Il file è aperto nell'IDE.

![](adding-a-controller/_static/image3.png)

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

I metodi del controller restituirà una stringa del codice HTML come esempio. Il controller è denominato `HelloWorldController` e il primo metodo riportato sopra è denominato `Index`. È possibile richiamarla da un browser. Eseguire l'applicazione (premere F5 o CTRL+F5). Nel browser, accodare &quot;HelloWorld&quot; al percorso nella barra degli indirizzi. (Ad esempio, nell'illustrazione seguente, relativo `http://localhost:1234/HelloWorld.`) la pagina nel browser avrà un aspetto simile allo screenshot seguente. Nel metodo precedente, il codice restituito direttamente una stringa. È indicato il sistema deve restituire solo del codice HTML e ha sempre fatto!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC richiama le classi controller diverso (e i metodi di azione diverso in esse contenute) a seconda dell'URL in ingresso. La logica di routing URL predefinita utilizzata da ASP.NET MVC Usa un formato simile al seguente per determinare quale codice per richiamare:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Così */HelloWorld* esegue il mapping al `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione per la classe da eseguire. Così */HelloWorld/Index* causerebbe il `Index` metodo del `HelloWorldController` classe da eseguire. Si noti che è necessario solo passare a */HelloWorld* e il `Index` metodo utilizzato per impostazione predefinita. Infatti, un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione iniziale... &quot;. Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image5.png)

È possibile modificare leggermente l'esempio in modo che è possibile passare le informazioni dei parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo da includere due parametri, come illustrato di seguito. Si noti che il codice Usa la funzionalità di c#-parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il [sistema di associazione di modelli ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping di parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image6.png)

In entrambi questi esempi il controller di elaborazione di operazioni i &quot;VC&quot; parte di MVC, vale a dire, il lavoro di visualizzazione e controller. Il controller restituisce direttamente l'HTML. In genere è preferibile non controller restituisce direttamente, l'HTML dal momento che diventa molto complessa al codice. Ma in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Si esaminerà successivo al modo in cui è possibile farlo.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-4.md)
> [Successivo](adding-a-view.md)
