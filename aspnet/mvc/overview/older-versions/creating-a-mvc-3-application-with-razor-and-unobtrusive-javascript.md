---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un'applicazione MVC 3 con Razor e JavaScript discreto Documenti Microsoft
author: rick-anderson
description: L'applicazione Web di esempio Elenco utenti dimostra quanto sia semplice creare ASP.NET applicazioni MVC 3 usando il motore di visualizzazione Razor.The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine. L'applicazione di esempio s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: c57e19f8eeca15e3676b3d490b08f69786d44f93
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542456"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Creazione di un'applicazione MVC 3 con Razor e JavaScript discreto

da parte [di Microsoft](https://github.com/microsoft)

> L'applicazione Web di esempio Elenco utenti dimostra quanto sia semplice creare ASP.NET applicazioni MVC 3 usando il motore di visualizzazione Razor.The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine. L'applicazione di esempio mostra come usare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web fittizio elenco utenti che include funzionalità quali la creazione, la visualizzazione, la modifica e l'eliminazione di utenti.
> 
> Questa esercitazione descrive i passaggi eseguiti per compilare l'esempio di elenco utenti ASP.NETapplicazione MVC 3. Per accompagnare questo argomento è disponibile un progetto di Visual Studio con il codice sorgente di C e VB: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Se hai domande su questo tutorial, puoi pubblicarli nel [forum MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Panoramica

L'applicazione che verrà costruito è un semplice sito web elenco utenti. Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

È possibile scaricare il progetto di VB e C, completato [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto utilizzando il modello *di applicazione Web ASP.NET MVC 3.* Denominare &quot;l'applicazione&quot;Mvc3Razor .

[![Nuovo progetto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Nella finestra di dialogo **Nuovo ASP.NET progetto MVC 3** selezionare Applicazione **Internet**, selezionare il motore di visualizzazione Razor e quindi fare clic su **OK**.

![Nuova finestra di dialogo Progetto MVC 3 ASP.NET](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In questa esercitazione non si utilizzerà il provider di appartenenze ASP.NET, pertanto è possibile eliminare tutti i file associati all'accesso e all'appartenenza. In **Esplora soluzioni**rimuovere i file e le directory seguenti:

- *Controller Di Controllo (Controllers/AccountController)*
- *Modelli: AccountModel*
- *Viste -\\_LogOnPartial condivise*
- *Visualizzazioni- Account* (e tutti i file in questa directory)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modificare `<div>` il `logindisplay` <em>&quot;</em>&quot; <em> \_file Layout.cshtml</em> e sostituire il markup all'interno dell'elemento denominato con il messaggio Login Disabled . L'esempio seguente mostra il nuovo markup:The following example shows the new markup:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Aggiunta del modello

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *Modelli* , scegliere **Aggiungi**, quindi **Classe**.

![Classe New User Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Denominare la classe `UserModel`. Sostituire il contenuto del file *UserModel* con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La `UserModel` classe rappresenta gli utenti. Ogni membro della classe è annotato con l'attributo [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dello spazio dei nomi [DataAnnotations.](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Gli attributi nello spazio dei nomi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) forniscono la convalida automatica lato client e lato server per le applicazioni Web.

Aprire `HomeController` la classe `using` e aggiungere una direttiva in modo da poter accedere alle classi `UserModel` e `Users` :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Subito dopo `HomeController` la dichiarazione, aggiungere il `Users` commento seguente e il riferimento a una classe:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La `Users` classe è un archivio dati in memoria semplificato che verrà usato in questa esercitazione. In un'applicazione reale si utilizzerebbe un database per archiviare le informazioni utente. Nell'esempio seguente `HomeController` vengono illustrate le prime righe del file:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compilare l'applicazione in modo che il modello utente sia disponibile per la procedura guidata di scaffolding nel passaggio successivo.

## <a name="creating-the-default-view"></a>Creazione della vista di default

Il passaggio successivo consiste nell'aggiungere un metodo di azione e una visualizzazione per visualizzare gli utenti.

Eliminare il file *esistente Views,Home-Index.* Verrà creato un nuovo file *di indice* per visualizzare gli utenti.

Nella `HomeController` classe sostituire il contenuto `Index` del metodo con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Fare clic con `Index` il pulsante destro del mouse all'interno del metodo, quindi **scegliere Aggiungi visualizzazione**.

![Aggiungi vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Selezionare l'opzione **Crea una vista fortemente tipata.** Per **Visualizza classe dati**, selezionare **Mvc3Razor.Models.UserModel**. Se **mvc3Razor.Models.UserModel** non viene visualizzato nella casella **Visualizza classe di dati,** è necessario compilare il progetto. Assicurarsi che il motore di visualizzazione sia impostato su **Razor**. Impostare **Visualizza contenuto** su **Elenco,** quindi fare clic su **Aggiungi**.

![Aggiungi visualizzazione indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nuova visualizzazione esegue automaticamente lo scaffolding `Index` dei dati utente passati alla visualizzazione. Esaminare il file *Views,Home o Index* appena generato. I collegamenti **Crea nuovo**, **Modifica**, **Dettagli**ed Eliminazione non **funzionano,** ma il resto della pagina è funzionale. Eseguire la pagina. Viene visualizzato un elenco di utenti.

![Pagina Indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Aprire il file *Index.cshtml* e sostituire `ActionLink` il markup per **Modifica**, **Dettagli**ed **Elimina** con il codice seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Il nome utente viene utilizzato come ID per trovare il record selezionato nei collegamenti **Modifica**, **Dettagli**ed **Elimina.**

## <a name="creating-the-details-view"></a>Creazione della vista Dettagli

Il passaggio successivo `Details` consiste nell'aggiungere un metodo di azione e una visualizzazione per visualizzare i dettagli dell'utente.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Aggiungere il `Details` seguente metodo al controller principale:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Fare clic con `Details` il pulsante destro del mouse all'interno del metodo, quindi <strong>scegliere Aggiungi visualizzazione</strong>. Verificare che la casella <strong>Visualizza classe di dati</strong> contenga <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Impostare <strong>Visualizza contenuto</strong> su <strong>Dettagli</strong> e quindi fare clic su <strong>Aggiungi</strong>.

![Aggiungere la visualizzazione dei dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Eseguire l'applicazione e selezionare un collegamento ai dettagli. Lo scaffolding automatico mostra ogni proprietà nel modello.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Creazione della vista di modifica

Aggiungere il `Edit` metodo seguente al controller principale.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Aggiungere una visualizzazione come nei passaggi precedenti, ma impostare **Visualizza contenuto** su **Modifica**.

![Aggiungi visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Eseguire l'applicazione e modificare il nome e il cognome di uno degli utenti. Se si violano i `DataAnnotation` vincoli `UserModel` applicati alla classe, quando si invia il modulo verranno visualizzati errori di convalida generati dal codice server. Ad esempio, se si &quot;modifica&quot; &quot;il&quot;nome Ann in A , quando si invia il modulo, nel modulo verrà visualizzato il seguente errore:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In questa esercitazione il nome utente verrà trattato come chiave primaria. Pertanto, la proprietà del nome utente non può essere modificata. Nel file *Edit.cshtml,* subito `Html.BeginForm` dopo l'istruzione, impostare il nome utente come campo nascosto. In questo modo la proprietà da passare nel modello. Il frammento di codice `Hidden` seguente mostra la posizione dell'istruzione:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Sostituire `TextBoxFor` il `ValidationMessageFor` e markup per `DisplayFor` il nome utente con una chiamata. Il `DisplayFor` metodo visualizza la proprietà come elemento di sola lettura. Nell'esempio seguente viene illustrato il markup completato. `TextBoxFor` L'originale `ValidationMessageFor` e le chiamate sono commentati con i caratteri`@* *@`di commento di inizio e fine Razor ( )

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Abilitazione della convalida lato clientEnabling Client-Side Validation

Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.

Aprire il file *Web.config* dell'applicazione. `that ClientValidationEnabled` Verificare `UnobtrusiveJavaScriptEnabled` e sono impostati su true nelle impostazioni dell'applicazione. Nel frammento seguente del file *Web.config* radice vengono visualizzate le impostazioni corrette:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

L'impostazione `UnobtrusiveJavaScriptEnabled` su true consente la convalida client Ajax discreta e discreta. Quando si utilizza la convalida discreta, le regole di convalida vengono trasformate in attributi HTML5. I nomi degli attributi HTML5 possono essere costituiti solo da lettere minuscole, numeri e trattini.

L'impostazione `ClientValidationEnabled` su true consente la convalida lato client. Impostando queste chiavi nel file *Web.config* dell'applicazione, si abilita la convalida client e JavaScript discreto per l'intera applicazione. È inoltre possibile abilitare o disabilitare queste impostazioni in singole visualizzazioni o nei metodi del controller utilizzando il codice seguente:You can also enable or disable these settings in individual views or in controller methods using the following code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

È inoltre necessario includere diversi file JavaScript nella visualizzazione sottoposta a rendering. Un modo semplice per includere il codice JavaScript in tutte le visualizzazioni consiste nell'aggiungerle al file *_Layout.cshtml\\di Views.* Sostituire `<head>` l'elemento del * \_file Layout.cshtml* con il codice seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

I primi due script jQuery sono ospitati dalla Microsoft Ajax Content Delivery Network (CDN). Sfruttando la rete CDN di Microsoft Ajax, è possibile migliorare significativamente le prestazioni di primo impatto delle applicazioni.

Eseguire l'applicazione e fare clic su un collegamento di modifica. Visualizzare l'origine della pagina nel browser. L'origine del browser mostra `data-val` molti attributi del modulo (per la convalida dei dati). Quando la convalida client e JavaScript discreto è abilitato, i campi `data-val="true"` di input con una regola di convalida client contengono l'attributo per attivare la convalida client discreta. Ad esempio, `City` il campo nel modello è stato decorato con l'attributo [Obbligatorio,](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) che determina il codice HTML illustrato nell'esempio seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Per ogni regola di convalida client, viene `data-val-rulename="message"`aggiunto un attributo con il modulo . Utilizzando `City` l'esempio di campo illustrato in precedenza, la regola di convalida client richiesta genera l'attributo `data-val-required` e il messaggio &quot;Il campo Città è obbligatorio.&quot; Eseguire l'applicazione, modificare uno degli `City` utenti e cancellare il campo. Quando si esce dalla scheda all'esterno del campo, viene visualizzato un messaggio di errore di convalida lato client.

![Città richiesta](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Analogamente, per ogni parametro nella regola di convalida client, viene aggiunto un attributo con il formato `data-val-rulename-paramname=paramvalue`. Ad esempio, `FirstName` la proprietà viene annotata con il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo e specifica una lunghezza minima di 3 e una lunghezza massima di 8. La regola di `length` convalida dei `max` dati denominata ha il nome del parametro e il valore del parametro 8.The data validation rule named has the parameter name and the parameter value 8. Di seguito viene illustrato il `FirstName` codice HTML generato per il campo quando si modifica uno degli utenti:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Per ulteriori informazioni sulla convalida client discreta, vedere la voce [Unabtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel blog di Brad Wilson.

> [!NOTE]
> In ASP.NET MVC 3 Beta, a volte è necessario inviare il modulo per avviare la convalida lato client. Questo potrebbe essere modificato per la versione finale.

## <a name="creating-the-create-view"></a>Creazione della vista Crea

Il passaggio successivo `Create` consiste nell'aggiungere un metodo di azione e una visualizzazione per consentire all'utente di creare un nuovo utente. Aggiungere il `Create` seguente metodo al controller principale:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Aggiungere una visualizzazione come nei passaggi precedenti, ma impostare **Visualizza contenuto** su **Crea**.

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Eseguire l'applicazione, selezionare il collegamento **Crea** e aggiungere un nuovo utente. Il `Create` metodo sfrutta automaticamente la convalida lato client e lato server. Provare a immettere un nome utente &quot;contenente&quot;spazi vuoti, ad esempio Ben X . Quando si esce dal campo del nome utente,`White space is not allowed`viene visualizzato un errore di convalida lato client ( ).

## <a name="add-the-delete-method"></a>Aggiungere il metodo Delete

Per completare l'esercitazione, aggiungere il metodo seguente `Delete` al controller principale:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Aggiungere `Delete` una visualizzazione come nei passaggi precedenti, impostando **Visualizza contenuto** su **Elimina**.

![Eliminare la visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Si dispone ora di un'applicazione MVC 3 ASP.NET semplice ma completamente funzionale con convalida.
