---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Aggiunta di un controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: ae3258872df798f52d8e031dc8ebf01b4f0a7358
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045130"
---
# <a name="adding-a-controller"></a>Aggiunta di un controller

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

MVC è l'acronimo di *Model-View-Controller*. MVC è un modello per lo sviluppo di applicazioni ben progettate, verificabili e facili da gestire. Le applicazioni basate su MVC contengono:

- **M** Odelli: classi che rappresentano i dati dell'applicazione e che utilizzano la logica di convalida per applicare le regole business per tali dati.
- **V** iste: file modello usati dall'applicazione per generare dinamicamente risposte HTML.
- **C** ontroller: classi che gestiscono le richieste del browser in ingresso, recuperano i dati del modello e quindi specificano i modelli di visualizzazione che restituiscono una risposta al browser.

Verranno trattati tutti questi concetti in questa serie di esercitazioni e verrà illustrato come usarli per creare un'applicazione.

Iniziamo creando una classe controller. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *controller* , quindi scegliere **Aggiungi**, quindi **controller**.

![](adding-a-controller/_static/image1.png)

Nella finestra di dialogo **Aggiungi impalcatura** scegliere **controller MVC 5-vuoto**e quindi fare clic su **Aggiungi**.

![](adding-a-controller/_static/image2.png)  

Assegnare un nome al nuovo controller "HelloWorldController" e fare clic su **Aggiungi**.

![Aggiungi controller](adding-a-controller/_static/image3.png)

Si noti che in **Esplora soluzioni** è stato creato un nuovo file denominato *HelloWorldController.cs* e una nuova cartella *Views\HelloWorld*. Il controller è aperto nell'IDE.

![](adding-a-controller/_static/image4.png)

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

I metodi del controller restituiranno una stringa di codice HTML come esempio. Il controller è denominato `HelloWorldController` e il primo metodo è denominato `Index` . Che verrà ora richiamato da un browser. Eseguire l'applicazione (premere F5 o CTRL + F5). Nel browser aggiungere &quot; HelloWorld &quot; al percorso nella barra degli indirizzi. (Ad esempio, nell'illustrazione seguente è `http://localhost:1234/HelloWorld.` ) La pagina nel browser sarà simile alla schermata seguente. Nel metodo precedente, il codice ha restituito direttamente una stringa. Si è detto al sistema di restituire solo codice HTML,

![](adding-a-controller/_static/image5.png)

ASP.NET MVC richiama diverse classi controller (e metodi di azione diversi al loro interno) a seconda dell'URL in ingresso. La logica di routing degli URL predefinita usata da ASP.NET MVC usa un formato simile al seguente per determinare il codice da richiamare:

`/[Controller]/[ActionName]/[Parameters]`

Il formato per il routing viene impostato nel file di * \_ avvio/RouteConfig. cs dell'app* .

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Quando si esegue l'applicazione e non si specifica alcun segmento URL, per impostazione predefinita viene impostato il controller "Home" e il metodo di azione "index" specificato nella sezione defaults del codice precedente.

La prima parte dell'URL determina la classe controller da eseguire. Quindi */HelloWorld* esegue il mapping alla `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione sulla classe da eseguire. Quindi */HelloWorld/index* provocherebbe l' `Index` esecuzione del metodo della `HelloWorldController` classe. Si noti che è stato necessario passare a */HelloWorld* e il `Index` metodo è stato usato per impostazione predefinita. Questo perché un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato uno in modo esplicito. La terza parte del segmento di URL ( `Parameters`) è relativa ai dati di route. I dati di route verranno visualizzati più avanti in questa esercitazione.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa che &quot; rappresenta il metodo di azione iniziale... &quot; . Il mapping MVC predefinito è `/[Controller]/[ActionName]/[Parameters]` . Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image6.png)

Modificare leggermente l'esempio in modo che sia possibile passare informazioni sui parametri dall'URL al controller, ad esempio */HelloWorld/Welcome? Name = Scott &amp; numtimes = 4*. Modificare il `Welcome` metodo in modo da includere due parametri, come illustrato di seguito. Si noti che il codice usa la funzionalità di parametro facoltativo C# per indicare che il `numTimes` parametro deve essere impostato su 1 se non viene passato alcun valore per il parametro.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Nota sulla sicurezza: il codice precedente USA [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) per proteggere l'applicazione da input dannoso (ovvero JavaScript). Per altre informazioni [, vedere Procedura: proteggere gli exploit di script in un'applicazione Web applicando la codifica HTML alle stringhe](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).

 Eseguire l'applicazione e passare all'URL di esempio ( `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` ). È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il [sistema di associazione di modelli MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image7.png)

Nell'esempio precedente, il segmento di URL ( `Parameters` ) non viene usato, i `name` `numTimes` parametri e vengono passati come [stringhe di query](http://en.wikipedia.org/wiki/Query_string). Il carattere ? (punto interrogativo) nell'URL precedente è un separatore e le stringhe di query seguono. Il carattere &amp; separa le stringhe di query.

Sostituire il metodo Welcome con il codice seguente:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Eseguire l'applicazione e immettere l'URL seguente: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Questa volta il terzo segmento URL corrisponde al parametro di route `ID.` il `Welcome` metodo di azione contiene un parametro ( `ID` ) che corrisponde alla specifica URL nel `RegisterRoutes` metodo.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Nelle applicazioni MVC ASP.NET, è più normale passare i parametri come dati di route (come abbiamo fatto con l'ID precedente) rispetto al passaggio come stringhe di query. È anche possibile aggiungere una route per passare i `name` parametri e `numtimes` in come dati di route nell'URL. Nel file * \_ Start\RouteConfig.cs dell'app* aggiungere la route "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Eseguire l'applicazione e passare a `/localhost:XXX/HelloWorld/Welcome/Scott/3` .

![](adding-a-controller/_static/image9.png)

Per molte applicazioni MVC, la route predefinita funziona correttamente. Si apprenderà più avanti in questa esercitazione per passare i dati usando lo strumento di associazione di modelli e non sarà necessario modificare la route predefinita.

In questi esempi il controller sta eseguendo la &quot; parte VC &quot; di MVC, ovvero la visualizzazione e il controller funzionano. Il controller restituisce direttamente l'HTML. In genere, non si vuole che i controller restituiscano direttamente il codice HTML, dal momento che diventa molto complesso da codificare. Si userà in genere un file modello di vista separato per facilitare la generazione della risposta HTML. Esaminiamo ora come possiamo eseguire questa operazione.

> [!div class="step-by-step"]
> [Precedente](getting-started.md) 
>  [Avanti](adding-a-view.md)
