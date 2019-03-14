---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: A livello di codice impostando i valori dei parametri di ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminato l'aggiunta di un metodo al nostro DAL e BLL che accetta un parametro di input e restituisce i dati. Questo parametro viene impostato nell'esempio...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f823d1db7f98dcbbef12d20df4a28e39fae0ac26
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062348"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Impostazione a livello di programmazione dei valori dei parametri di ObjectDataSource (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) o [Scarica il PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> In questa esercitazione verrà esaminato l'aggiunta di un metodo al nostro DAL e BLL che accetta un parametro di input e restituisce i dati. Questo parametro viene impostato a livello di codice nell'esempio.


## <a name="introduction"></a>Introduzione

Come abbiamo visto nel [esercitazione precedente](declarative-parameters-vb.md), una serie di opzioni è disponibile per il passaggio in modo dichiarativo i valori dei parametri ai metodi di ObjectDataSource. Se il valore del parametro è hardcoded, proviene da un controllo Web nella pagina o in qualsiasi altra origine che è leggibile da un'origine dati `Parameter` dell'oggetto, ad esempio, di valore può essere associato al parametro di input senza scrivere una riga di codice.

Potrebbero esserci casi, tuttavia, quando il valore del parametro viene fornito da un'origine non è già stato presi in considerazione da uno dei file di dati incorporato `Parameter` oggetti. Se gli account utente è supportato il nostro sito potrebbe essere necessario impostare il parametro di base attualmente connesso nell'ID del utente. visitatore dell' Oppure potrebbe essere necessario personalizzare il valore del parametro prima dell'invio insieme al metodo dell'oggetto sottostante di ObjectDataSource.

Ogni volta che ObjectDataSource `Select` metodo viene richiamato ObjectDataSource genera prima relativi [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Quindi viene richiamato il metodo dell'oggetto sottostante di ObjectDataSource. Dopo aver completato l'operazione di ObjectDataSource [selezionati eventi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) generato (figura 1 illustra questa sequenza di eventi). I valori del parametro passati nel metodo dell'oggetto sottostante di ObjectDataSource possono essere impostati o personalizzati in un gestore eventi per il `Selecting` evento.


[![Viene richiamato selezionati e il metodo selezionando gli eventi attivati prima e dopo il suo dell'oggetto sottostante di ObjectDataSource](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: ObjectDataSource `Selected` e `Selecting` metodo eventi attivati prima e dopo il suo sottostante dell'oggetto viene richiamato ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


In questa esercitazione verrà esaminato l'aggiunta di un metodo al nostro DAL e BLL che accetta un parametro di input `Month`, di tipo `Integer` e restituisce un `EmployeesDataTable` oggetto popolato con i dipendenti che hanno la ricorrenza annuale dell'assunzione nell'oggetto specificato `Month`. Questo esempio verrà impostato questo parametro a livello di programmazione basato sul mese corrente, che mostra un elenco di "Employee ricorrenze questo mese".

Iniziamo!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Passaggio 1: Aggiunta di un metodo per`EmployeesTableAdapter`

Per questo primo esempio è necessario aggiungere un mezzo per recuperare i dipendenti il cui `HireDate` si è verificato in un mese specificato. Per fornire questa funzionalità in base alla nostra architettura è necessario innanzitutto creare un metodo in `EmployeesTableAdapter` che esegue il mapping all'istruzione SQL appropriata. A tale scopo, iniziare aprendo il set di dati tipizzato Northwind. Fare clic su di `EmployeesTableAdapter` assegnare un'etichetta e scegliere Aggiungi Query.


[![Aggiungere una nuova Query per il EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: Aggiungere una nuova Query per il `EmployeesTableAdapter` ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Scegliere di aggiungere un'istruzione SQL che restituisce righe. Quando si raggiunge lo specificare una `SELECT` istruzione schermata il valore predefinito `SELECT` istruzione per il `EmployeesTableAdapter` saranno già caricato. Aggiungere semplicemente nel `WHERE` clausola: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) è una funzione T-SQL che restituisce una data specifica parte di un `datetime` tipo; in questo caso usiamo `DATEPART` per tornare il mese del `HireDate` colonna.


[![Restituito solo le righe in cui HireDate Sloupec JE minore o uguale al @HiredBeforeDate parametro](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: Restituire solo le righe in cui il `HireDate` colonna è minore o uguale al `@HiredBeforeDate` parametro ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Infine, modificare il `FillBy` e `GetDataBy` i nomi dei metodi per `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`, rispettivamente.


[![Scegliere i nomi dei metodi più Appropriate rispetto alle FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: Scegliere più appropriato metodo nomi rispetto `FillBy` e `GetDataBy` ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Fare clic su Fine per completare la procedura guidata e tornare all'area di progettazione del set di dati. Il `EmployeesTableAdapter` dovrebbe ora includere un nuovo set di metodi per l'accesso di dipendenti assunti in un mese specificato.


[![I nuovi metodi vengono visualizzati nell'area di progettazione del set di dati](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: I nuovi metodi presenti nell'area di progettazione del set di dati ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Passaggio 2: Aggiunta di`GetEmployeesByHiredDateMonth(month)`metodo a livello della logica di Business

Poiché l'Usa di architettura dell'applicazione separato dei livelli per la logica di business e dati di accesso per la logica, è necessario aggiungere un metodo per il livello BLL che le chiamate al per recuperare i dipendenti assunti prima della data specificata. Aprire il `EmployeesBLL.vb` file e aggiungere il metodo seguente:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Come con altri metodi in questa classe `GetEmployeesByHiredDateMonth(month)` chiama semplicemente verso il basso DAL e restituisce i risultati.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Passaggio 3: Visualizzazione dei dipendenti la cui anniversario assunzione è questo mese

Il passaggio finale per questo esempio consiste nella visualizzazione quei dipendenti la cui anniversario assunzione è questo mese. Iniziare aggiungendo un controllo GridView per il `ProgrammaticParams.aspx` nella pagina di `BasicReporting` cartella e aggiungere un nuovo oggetto ObjectDataSource come origine dati. Configurare ObjectDataSource per usare la `EmployeesBLL` classe con il `SelectMethod` impostato su `GetEmployeesByHiredDateMonth(month)`.


[![Usare la classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: Usare la `EmployeesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Selezionare da GetEmployeesByHiredDateMonth(month) (metodo)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: Select From la `GetEmployeesByHiredDateMonth(month)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Schermata finale viene chiesto di specificare per fornire la `month` origine del valore del parametro. Poiché questo valore verrà impostata a livello di codice, lasciare il parametro source impostati sul valore predefinito None opzione e fare clic su Fine.


[![Lasciare il parametro Source impostato su Nessuno](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: Lasciare l'origine di parametro impostato su None ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Verrà creato un `Parameter` oggetto nella finestra di ObjectDataSource `SelectParameters` raccolta che non ha un valore specificato.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Per impostare questo valore a livello di codice, è necessario creare un gestore eventi per ObjectDataSource `Selecting` evento. A tale scopo, passare alla visualizzazione progettazione e fare doppio clic su ObjectDataSource. In alternativa, selezionare ObjectDataSource, passare alla finestra delle proprietà e scegliere l'icona a forma di fulmine. Successivamente, fare doppio clic nella casella di testo accanto al `Selecting` evento o digitare il nome da usare il gestore dell'evento. Come terza opzione, è possibile creare il gestore dell'evento selezionando ObjectDataSource e i relativi `Selecting` evento due elenchi di riepilogo a discesa nella parte superiore della classe code-behind della pagina.


![Fare clic sull'icona a forma di fulmine nella finestra proprietà per visualizzare l'elenco degli eventi di un controllo Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: Fare clic sull'icona a forma di fulmine nella finestra proprietà per visualizzare l'elenco degli eventi di un controllo Web


Tutti i tre approcci aggiungere un nuovo gestore eventi per ObjectDataSource `Selecting` classe code-behind della pagina dell'evento. In questo gestore eventi è possibile leggere e scrivere i valori di parametro utilizzando `e.InputParameters(parameterName)`, dove *`parameterName`* è il valore della `Name` attributo il `<asp:Parameter>` tag (il `InputParameters` insieme può anche essere indicizzata ordinale, come in `e.InputParameters(index)`). Per impostare il `month` parametro per il mese corrente, aggiungere il codice seguente il `Selecting` gestore dell'evento:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Quando si visita questa pagina tramite un browser è possibile osservare che un solo dipendente è stato assunto questo mese (marzo) Laura Callahan, chi ha lavorato nell'azienda dal 1994.


[![I dipendenti il cui ricorrenze questo mese vengono visualizzati](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: I dipendenti la cui ricorrenze questo mese vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Riepilogo

Mentre i parametri di ObjectDataSource in genere possibile impostare i valori in modo dichiarativo, senza richiedere una riga di codice, è facile impostare i valori dei parametri a livello di codice. Tutto dobbiamo fare è creare un gestore eventi per ObjectDataSource `Selecting` evento, che viene attivato prima che venga richiamato il metodo dell'oggetto sottostante e impostare manualmente i valori per uno o più parametri tramite la `InputParameters` raccolta.

Questa esercitazione si conclude la sezione Basic Reporting. Il [prossima esercitazione](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) la sezione di operazioni di filtro e gli scenari di Master-Details, in cui abbiamo verranno analizzate le tecniche per consentire il visitatore per filtrare i dati e il drill-down da un report master in un report dettagli viene avviato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](declarative-parameters-vb.md)
