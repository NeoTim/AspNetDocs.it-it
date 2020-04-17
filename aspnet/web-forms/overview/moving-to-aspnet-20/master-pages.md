---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: 'Pagine master : Documenti Microsoft'
author: rick-anderson
description: Uno dei componenti chiave per un sito Web di successo è un aspetto coerente. In ASP.NET 1.x, gli sviluppatori hanno utilizzato i controlli utente per replicare le pagine comuni...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: b493feb21d2e3d6429f0a23df5aab66e0c4c5b07
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543184"
---
# <a name="master-pages"></a>Pagine master

da parte [di Microsoft](https://github.com/microsoft)

> Uno dei componenti chiave per un sito Web di successo è un aspetto coerente. In ASP.NET 1.x, gli sviluppatori hanno utilizzato i controlli utente per replicare gli elementi comuni della pagina in un'applicazione Web. Anche se questa è certamente una soluzione praticabile, l'utilizzo dei controlli utente ha alcuni svantaggi. Ad esempio, una modifica nella posizione di un controllo utente richiede una modifica a più pagine in un sito. Anche i controlli utente non vengono sottoposti a rendering nella visualizzazione Progettazione dopo l'inserimento in una pagina.

Uno dei componenti chiave per un sito Web di successo è un aspetto coerente. In ASP.NET 1.x, gli sviluppatori hanno utilizzato i controlli utente per replicare gli elementi comuni della pagina in un'applicazione Web. Anche se questa è certamente una soluzione praticabile, l'utilizzo dei controlli utente ha alcuni svantaggi. Ad esempio, una modifica nella posizione di un controllo utente richiede una modifica a più pagine in un sito. Anche i controlli utente non vengono sottoposti a rendering nella visualizzazione Progettazione dopo l'inserimento in una pagina.

ASP.NET 2.0 introduce le pagine master come un modo per mantenere un aspetto coerente e, come si vedrà presto, le pagine master rappresentano un miglioramento significativo rispetto al metodo di controllo utente.

## <a name="why-master-pages"></a>Perché le pagine master?

Ci si potrebbe chiedere perché le pagine master erano necessarie in ASP.NET 2.0. Dopo tutto, gli sviluppatori di siti Web utilizzano già i controlli utente in ASP.NET 1.x per condividere aree di contenuto tra le pagine. Esistono in realtà diversi motivi per cui i controlli utente sono una soluzione non ottimale per la creazione di un layout comune.

I controlli utente non definiscono effettivamente il layout di pagina. Al contrario, definiscono il layout e la funzionalità per una parte di una pagina. La distinzione tra questi due è importante perché rende la gestibilità di una soluzione di controllo utente molto più difficile. Ad esempio, quando si desidera modificare la posizione di un controllo utente nella pagina, è necessario modificare la pagina effettiva in cui viene visualizzato il controllo utente. Va bene se hai solo poche pagine, ma in siti di grandi dimensioni, diventa rapidamente un incubo di gestione del sito!

Un altro svantaggio dell'utilizzo dei controlli utente per la definizione di un layout comune è radicato nell'architettura di ASP.NET stesso. Se un membro pubblico di un controllo utente viene modificato, è necessario ricompilare tutte le pagine che utilizzano il controllo utente. A sua volta, ASP.NET quindi re-JIT le pagine quando si accede per la prima volta. Questo, ancora una volta, produce un'architettura non scalabile e un problema di gestione del sito per i siti più grandi.

Entrambi questi problemi (e molti altri) sono ben affrontati dalle pagine master in ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Funzionamento delle pagine master

Una pagina master è analoga a un modello per altre pagine. Gli elementi di pagina che devono essere condivisi tra le altre pagine (ad esempio menu, bordi e così via) vengono aggiunti alla pagina master. Quando vengono aggiunte nuove pagine al sito, è possibile associarle a una pagina master. Una pagina associata a una pagina master viene definita pagina di **contenuto.** Per impostazione predefinita, una pagina di contenuto assume l'aspetto della pagina master. Tuttavia, quando si crea una pagina master, è possibile definire parti della pagina che la pagina di contenuto può sostituire con il proprio contenuto. Queste parti vengono definite utilizzando un nuovo controllo introdotto in ASP.NET 2.0; il controllo **ContentPlaceHolder.**

Una pagina master può contenere un numero qualsiasi di controlli ContentPlaceHolder (o nessuno). Nella pagina di contenuto, il contenuto dai controlli ContentPlaceHolder viene visualizzato all'interno di **controlli contenuto,** un altro nuovo controllo in ASP.NET 2.0. Per impostazione predefinita, i controlli contenuto delle pagine di contenuto sono vuoti in modo che sia possibile fornire contenuto personalizzato. Se si desidera utilizzare il contenuto della pagina master all'interno dei controlli contenuto, è possibile farlo come si vedrà più avanti in questo modulo. Il controllo Content è mappato al controllo ContentPlaceHolder tramite l'attributo ContentPlaceHolderID del controllo Content. Il codice seguente esegue il mapping di un controllo Content a un controllo ContentPlaceHolder denominato mainBody in una pagina master.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Spesso si sente le persone descrivono le pagine master come una classe base per altre pagine. Questo in realtà non è vero. La relazione tra le pagine master e le pagine di contenuto non è un rapporto di ereditarietà.

**Figura 1** Mostra una pagina master e una pagina di contenuto associato come vengono visualizzati in Visual Studio 2005. È possibile visualizzare il controllo ContentPlaceHolder nella pagina master e il controllo contenuto corrispondente nella pagina di contenuto. Si noti che il contenuto delle pagine master che si trova all'esterno di ContentPlaceHolder è visibile ma in grigio nella pagina di contenuto. Solo il contenuto all'interno di ContentPlaceHolder può essere sostituito dalla pagina di contenuto. Tutto il contenuto proveniente dalla pagina master non è modificabile.

![Una pagina master e la pagina di contenuto associata](master-pages/_static/image1.jpg)

**Figura 1:** una pagina master e la pagina di contenuto associata

## <a name="creating-a-master-page"></a>Creazione di una pagina masterCreating a Master Page

Per creare una nuova pagina master:

1. Aprire Visual Studio 2005 e creare un nuovo sito Web.
2. Fare clic su File, Nuovo, File.
3. Scegliere File principale dalla finestra di dialogo Aggiungi nuovo elemento, come illustrato nella **figura 2.**
4. Fare clic su Aggiungi.

![Creazione di una nuova pagina masterCreating a New Master Page](master-pages/_static/image2.jpg)

**Figura 2:** Creazione di una nuova pagina masterFigure 2 : Creating a New Master Page

Si noti che l'estensione di file per una pagina master è *.master*. Questo è uno dei modi in cui una pagina master differisce da una pagina normale. L'altra differenza principale è che @Page al posto di @Master una direttiva, la pagina master contiene una direttiva. Passare alla visualizzazione Origine per la pagina master appena creata ed esaminare il codice.

Una nuova pagina master avrà un controllo ContentPlaceHolder per impostazione predefinita. Nella maggior parte dei casi, ha più senso creare prima gli elementi comuni della pagina e quindi inserire i controlli ContentPlaceHolder in cui si desidera il contenuto personalizzato. In questi casi, gli sviluppatori vorranno eliminare il controllo ContentPlaceHolder predefinito e inserirne di nuovi durante lo sviluppo della pagina. I controlli ContentPlaceHolder non sono ridimensionabili nonostante visualizzino i quadratini di ridimensionamento. Il controllo ContentPlaceHolder viene ridimensionato automaticamente in base al contenuto in esso contenuto con un'eccezione; Se si inserisce un controllo ContentPlaceHolder all'interno di un elemento di blocco, ad esempio una cella di tabella, verrà ridimensionato in base alle dimensioni dell'elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratorio 1 Utilizzo delle pagine master

In questa esercitazione verrà creata una nuova pagina master e verranno definiti tre controlli ContentPlaceHolder.In this lab, you will create a new master page and define three ContentPlaceHolder controls. Si creerà quindi una nuova pagina di contenuto e sostituire il contenuto da almeno uno dei controlli ContentPlaceHolder.

1. Creare una pagina master e inserire i controlli ContentPlaceHolder.Create a master page and insert ContentPlaceHolder controls. 

    1. Creare una nuova pagina master come descritto in precedenza.
    2. Eliminare il controllo ContentPlaceHolder predefinito.
    3. Selezionare il controllo ContentPlaceHolder facendo clic sul bordo superiore ombreggiato del controllo e quindi eliminarlo premendo il tasto CANC sulla tastiera.
    4. Inserire una nuova tabella utilizzando *l'intestazione e* il modello laterale come illustrato nella figura 3. Modificare la larghezza e l'altezza al 90% di ciascuna in modo che l'intera tabella sia visibile nella finestra di progettazione.

![](master-pages/_static/image3.jpg)

**Figura 3**

1. Posizionare il cursore in ogni cella della tabella e impostare la proprietà *valign* su *top*.
2. Dalla casella degli strumenti, inserire un controllo ContentPlaceHolder nella cella superiore della tabella (la cella di intestazione).
3. Quando si inserisce questo controllo ContentPlaceHolder, si noterà che l'altezza della riga occuperà quasi l'intera pagina, come illustrato nella figura 4. Non preoccupatevi di questo a questo punto.

![Lo spazio vuoto si trova nella stessa cella di ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4:** lo spazio vuoto si trova nella stessa cella di ContentPlaceHolder

1. Inserire un controllo ContentPlaceHolder nelle altre due celle. Dopo aver inserito gli altri controlli ContentPlaceHolder, le dimensioni delle celle della tabella devono essere quelle previste. La pagina dovrebbe ora essere simile alla pagina illustrata nella **figura 5**.

![Il Master con tutti i controlli ContentPlaceHolder.The Master with all ContentPlaceHolder controls. Si noti che l'altezza della cella per la cella di intestazione è ora quella che dovrebbe essere](master-pages/_static/image2.gif)

**Figura 5**: il master con tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per la cella di intestazione è ora quella che dovrebbe essere

1. Immettere il testo desiderato in ognuno dei tre controlli ContentPlaceHolder.
2. Salvate la pagina master come exercise1.master.
3. Creare un nuovo Web Form e associarlo alla pagina master exercise1.master.
4. Selezionare File, Nuovo, File in Visual Studio 2005.
5. Selezionare **Web Form** nella finestra di dialogo Aggiungi nuovo elemento.
6. Assicurarsi che la casella di controllo Seleziona pagina master sia selezionata come illustrato nella figura 6.

![Aggiunta di una nuova pagina di contenuto](master-pages/_static/image3.gif)

**Figura 6:** Aggiunta di una nuova pagina di contenuto

1. Fare clic su Aggiungi.
2. Selezionate exercise1.master nella finestra di dialogo Seleziona una pagina master come illustrato nella figura 7.
3. Fare clic su OK per aggiungere la nuova pagina di contenuto.

La nuova pagina di contenuto viene visualizzata in Visual Studio con un controllo contenuto per ogni controllo ContentPlaceHolder nella pagina master. Per impostazione predefinita, i controlli contenuto sono vuoti in modo che è possibile aggiungere il proprio contenuto. Se si desidera che utilizzino il contenuto del controllo ContentPlaceHolder nella pagina master, è sufficiente fare clic sul simbolo dello smart tag (la piccola freccia nera nell'angolo superiore destro del controllo) e scegliere *Predefinito il contenuto Master* dallo smart tag, come illustrato nella figura **8**. In questo caso, la voce di menu diventa *Crea contenuto personalizzato*. Facendo clic su di esso a quel punto rimuove il contenuto dalla pagina master che consente di definire contenuto personalizzato per quel particolare controllo contenuto.

![Impostazione di un controllo contenuto predefinito per il contenuto delle pagine masterSetting a Content Control to Default to the Master Pages Content](master-pages/_static/image4.gif)

Figura 7 : Impostazione di un controllo contenuto predefinito per il contenuto delle pagine **masterFigure 7**: Setting a Content Control to Default to the Master Pages Content

## <a name="connecting-master-page-and-content-pages"></a>Connessione di pagine master e pagine di contenutoConnecting Master Page and Content Pages

L'associazione tra una pagina master e una pagina di contenuto può essere configurata in uno dei quattro modi seguenti:The association between a master page and a content page can be configured in one of four different ways:

- L'attributo <strong>MasterPageFile</strong> della @Page direttiva
- Impostazione della proprietà **Page.MasterPageFile** nel codice.
- L'elemento ** &lt;pages&gt; ** nel file di configurazione delle applicazioni (web.config nella cartella radice dell'applicazione)
- Elemento ** &lt;&gt; pages** in un file di configurazione delle sottocartelle (web.config in una sottocartella)

## <a name="masterpagefile-attribute"></a>Attributo MasterPageFile

Il MasterPageFile attributo semplifica l'applicazione di una pagina master a una determinata ASP.NET pagina. È anche il metodo utilizzato per applicare la pagina master quando si seleziona la casella di controllo **Seleziona pagina master** come nell'esercizio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Impostazione di Page.MasterPageFile nel codiceSetting Page.MasterPageFile in Code

Impostando la proprietà MasterPageFile nel codice, è possibile applicare una determinata pagina master al contenuto in fase di esecuzione. Ciò è utile nei casi in cui potrebbe essere necessario applicare una pagina master specifica in base a un ruolo di utenti o ad altri criteri. Il MasterPageFile proprietà deve essere impostata nel PreInit metodo. Se viene impostato dopo il preInit metodo, un InvalidOperationException verrà generata. La pagina in cui viene impostata questa proprietà deve inoltre disporre di un controllo contenuto come controllo di primo livello per la pagina. In caso contrario, verrà generata un'eccezione HttpException quando viene impostata la proprietà MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Utilizzo &lt;dell'elemento pages&gt;

È possibile configurare una pagina master per le pagine &lt;&gt; impostando l'attributo masterPageFile nell'elemento pages del file web.config. Quando si utilizza questo metodo, tenere presente che i file web.config più in basso nella struttura dell'applicazione possono eseguire l'override di questa impostazione. Qualsiasi attributo MasterPageFile @Page impostato in una direttiva eseguirà anche l'override di questa impostazione. L'utilizzo dell'elemento &lt;pages&gt; semplifica la creazione di una pagina *master* che può essere sostituita se necessario in determinate cartelle o file.

## <a name="properties-in-master-pages"></a>Proprietà nelle pagine master

Una pagina master può esporre le proprietà semplicemente rendendo tali proprietà pubbliche all'interno della pagina master. Ad esempio, il codice seguente definisce una proprietà denominata SomeProperty:For example, the following code defines a property called SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Per accedere alla proprietà SomeProperty dalla pagina Contenuto, è necessario utilizzare la proprietà Master in questo modo:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Nidificazione di pagine master

Le pagine master sono la soluzione perfetta per garantire un aspetto comune in un'applicazione Web di grandi dimensioni. Tuttavia, non è raro che alcune parti di un sito di grandi dimensioni condividano un'interfaccia comune, mentre altre parti condividono un'interfaccia diversa. Per soddisfare questa esigenza, più pagine master sono la soluzione perfetta. Tuttavia, che ancora non affronta il fatto che un'applicazione di grandi dimensioni può avere alcuni componenti (ad esempio un menu) che sono condivisi tra tutte le pagine e altri componenti che sono condivisi solo tra alcune sezioni del sito. Per questo tipo di situazione, le pagine master annidate riempiono bene la necessità. Come si è visto, una pagina master normale è costituita da una pagina master e una pagina di contenuto. In una situazione di pagina master annidata, sono presenti due pagine master; un master padre e un master figlio. La pagina master figlio è anche una pagina di contenuto e il relativo master è la pagina master padre.

Ecco il codice per una tipica pagina master:Here is the code for a typical master page:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In uno scenario master annidato, questo sarebbe il master padre. Un'altra pagina master userebbe questa pagina come pagina master e tale codice sarebbe simile al seguente:An master page would use this page as its master page, and that code would look like this:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Si noti che in questo scenario, il master figlio è anche una pagina di contenuto per il master padre. Tutto il contenuto del master figlio viene visualizzato all'interno di un controllo Contenuto che ottiene il contenuto dal controllo ContentPlaceHolder dell'elemento padre.

> [!NOTE]
> Il supporto della finestra di progettazione non è disponibile per le pagine master annidate. Quando si sviluppa utilizzando master annidati, è necessario utilizzare la visualizzazione origine.

In questo video viene illustrata una procedura dettagliata sull'utilizzo di pagine master annidate.

![](master-pages/_static/image1.png)

[Apri video a schermo intero](master-pages/_static/nested1.wmv)

![Selezione di una pagina master](master-pages/_static/image4.jpg)

**Figura 8**: Selezione di una pagina master
