---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Come si usa il controllo ComboBox? (VB) Documenti Microsoft
author: rick-anderson
description: ComboBox è un ASP.NET controllo AJAX che combina la flessibilità di un controllo TextBox con un elenco di opzioni tra cui gli utenti possono scegliere.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 237e3ef864238c11fc1fb49239c3f6fa3f75537d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543067"
---
# <a name="how-do-i-use-the-combobox-control-vb"></a>Come si usa il controllo ComboBox? (VB)

da parte [di Microsoft](https://github.com/microsoft)

> ComboBox è un ASP.NET controllo AJAX che combina la flessibilità di un controllo TextBox con un elenco di opzioni tra cui gli utenti possono scegliere.

L'obiettivo di questa esercitazione è spiegare il controllo ComboBox di AJAX Control Toolkit. Il controllo ComboBox funziona come una combinazione tra un controllo DropDownList standard ASP.NET e un controllo TextBox. È possibile selezionare da un elenco di elementi preesistente o immettere un nuovo elemento.

Il controllo ComboBox è simile all'estensione del controllo AutoComplete, ma i controlli vengono utilizzati in scenari diversi. L'estensione Completamento automatico esegue una query su un servizio Web per ottenere voci corrispondenti. Il controllo ComboBox, al contrario, viene inizializzato con un set di elementi. L'utilizzo dell'estensione AutoComplete ha senso quando si lavora con un ampio set di dati (milioni di parti di auto) durante l'utilizzo del controllo ComboBox ha senso quando si lavora con un piccolo set di dati (decine di parti di auto).

## <a name="selecting-from-a-static-list-of-items"></a>Selezione da un elenco statico di elementi

Iniziate s con un semplice esempio di utilizzo del controllo ComboBox. Si supponga di voler visualizzare un elenco statico di elementi in un elenco a discesa. Tuttavia, si desidera lasciare aperta la possibilità che l'elenco non sia completo. Si desidera consentire a un utente di immettere un valore personalizzato nell'elenco.

Verrà creata una nuova pagina ASP.NET Web Form e verrà utilizzato il controllo ComboBox nella pagina. Aggiungere la nuova pagina ASP.NET al progetto e passare alla visualizzazione Progettazione.

Se si desidera utilizzare il controllo ComboBox nella pagina, è necessario aggiungere un controllo ScriptManager alla pagina. Trascinare il controllo ScriptManager dalla sotto la scheda Estensioni AJAX nell'area di progettazione. È necessario aggiungere il controllo ScriptManager nella parte superiore della pagina; è possibile aggiungerlo immediatamente sotto &lt;il&gt; tag del modulo lato server di apertura.

Successivamente, trascinare il controllo ComboBox nella pagina. È possibile trovare il controllo ComboBox nella casella degli strumenti con gli altri controlli AJAX Control Toolkit ed estensioni di controllo (vedere figura1).

[![Modulo semplice per la creazione di un biglietto da visita](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figura 01**: Selezione del controllo ComboBox dalla casella degli strumenti ([Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image2.png)a dimensioni intere)

Utilizzeremo il controllo ComboBox per visualizzare un elenco statico di scelte. L'utente può selezionare un particolare livello di piccantezza per il proprio cibo da un elenco di tre opzioni: Mild, Medium e Hot (vedere Figura 2).

[![Selezione da un elenco statico di elementi](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figura 02**: Selezione da un elenco statico di elementi([Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image4.png)a dimensioni intere)

Esistono due modi per aggiungere queste scelte al controllo ComboBox. In primo luogo, si seleziona l'opzione di attività Modifica opzioni quando si passa il mouse sul controllo in visualizzazione Progettazione e si apre l'Editor di elementi (vedere Figura 3).

[![Modifica degli elementi comboBox](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figura 03**: Modifica degli elementi comboBox([Fare clic per visualizzare l'immagine a dimensioni intere)](how-do-i-use-the-combobox-control-vb/_static/image6.png)

La seconda opzione consiste nell'aggiungere l'elenco &lt;di elementi&gt; tra i tag asp:ComboBox di apertura e chiusura nella visualizzazione Origine. La pagina nel listato 1 contiene il controllo ComboBox aggiornato con l'elenco di elementi.

**Listato 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Quando si apre la pagina nel listato 1, è possibile selezionare una delle opzioni preesistenti dal ComboBox. In altre parole, il ComboBox funziona come un controllo DropDownList.

Tuttavia, hai anche la possibilità di inserire una nuova scelta (ad esempio, Super Spicy) che non è nell'elenco esistente. Così, il ComboBox funziona anche come un TextBox controllo.

Indipendentemente dal fatto che si seleziona un elemento preesistente o si immette un elemento personalizzato, quando si invia il modulo, la scelta viene visualizzata nel controllo etichetta. Quando si invia il modulo,\_il btnSubmit Click gestore esegue e aggiorna l'etichetta (vedere Figura 4).

[![Visualizzazione dell'elemento selezionato](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figura 04**: Visualizzazione dell'elemento[selezionato(Fare clic per visualizzare l'immagine a dimensioni intere)](how-do-i-use-the-combobox-control-vb/_static/image8.png)

Il controllo ComboBox supporta le stesse proprietà del controllo DropDownList per il recupero dell'elemento selezionato dopo l'invio di un form:

- SelectedItem.Text - Visualizza il valore della proprietà Text dell'elemento selezionato.
- SelectedItem.Value - Visualizza il valore della proprietà Value dell'elemento selezionato o visualizza il testo digitato nel controllo ComboBox.
- SelectedValue - Uguale a SelectedItem.Value, ad eccezione del fatto che questa proprietà consente di specificare l'elemento selezionato predefinito (iniziale).

Se si digita una scelta personalizzata nel controllo ComboBox, la scelta personalizzata viene assegnata a entrambe le proprietà SelectedItem.Text e SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selezione dell'elenco di elementi dal database

È possibile recuperare l'elenco di elementi visualizzati da ComboBox da un database. Ad esempio, è possibile associare il controllo ComboBox a un controllo SqlDataSource, un controllo ObjectDataSource, LinqDataSource o un oggetto EntityDataSource.For example, you can bind the ComboBox to a SqlDataSource control, an ObjectDataSource control, a LinqDataSource, or an EntityDataSource.

Si supponga di voler visualizzare un elenco di filmati in un controllo ComboBox.Immaginate che si desidera visualizzare un elenco di film in un ComboBox.Imagine that you want to display a list of movies in a ComboBox. Si desidera recuperare l'elenco dei film dalla tabella di database Movies. A tale scopo, seguire questa procedura:

1. Creare una pagina denominata Movies.aspxCreate a page named Movies.aspx
2. Aggiungere un controllo ScriptManager alla pagina trascinando ScriptManager dalla scheda Estensioni AJAX della Casella degli strumenti nella pagina.
3. Aggiungere un controllo ComboBox alla pagina trascinando il controllo ComboBox nella pagina.
4. In visualizzazione Progettazione passare il mouse sul controllo ComboBox e selezionare l'opzione di attività **Scegli origine dati** (vedere la Figura 5). Viene avviata la Configurazione guidata origine dati.
5. Nel passaggio **Scegliere un'origine dati** selezionare l'opzione &lt;Nuova origine&gt; dati.
6. Nel passaggio Scegliere un tipo di origine dati selezionare Database.In the Choose a **Data Source Type step,** select Database.
7. Nel passaggio Scegliere la **connessione dati** selezionare il database, ad esempio MoviesDB.mdf.
8. Nel passaggio Salva la stringa di connessione nel file di **configurazione dell'applicazione** selezionare l'opzione per salvare la stringa di connessione.
9. Nel passaggio **Configura istruzione Select** selezionare la tabella di database Movies e tutte le colonne.
10. Nel passaggio **Test query** fare clic sul pulsante Fine.
11. Nel passaggio **Scegli origine dati** selezionare la colonna Titolo per il campo da visualizzare e la colonna Id per il campo dati (vedere la figura).
12. Fare clic sul pulsante OK per chiudere la procedura guidata.

[![Scelta di un'origine datiChoosing a data source](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figura 05**: Scelta di un'origine[dati(Fare clic per visualizzare l'immagine a dimensioni intere)](how-do-i-use-the-combobox-control-vb/_static/image10.png)

[![Scelta del testo dei dati e dei campi valore](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figura 06**: Scelta del testo dei dati e dei campi[valore(Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image12.png)a dimensioni intere)

Dopo aver completato i passaggi precedenti, il controllo ComboBox viene associato a un controllo SqlDataSource che rappresenta i filmati dalla tabella di database Movies. L'origine della pagina è simile al listato 2 (ho pulito un po' la formattazione).

**Listato 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Si noti che il controllo ComboBox dispone di una proprietà DataSourceID che punta al controllo SqlDataSource. Quando si apre la pagina in un browser, viene visualizzato l'elenco dei filmati dal database (vedere Figura 7). È possibile selezionare un filmato dall'elenco o immettere un nuovo filmato digitando il filmato nel controllo ComboBox.

[![Visualizzazione di un elenco di filmati](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figura 07**: Visualizzazione di un elenco di[filmati(Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image14.png)a dimensioni intere)

## <a name="setting-the-dropdownstyle"></a>Impostazione di DropDownStyle

È possibile utilizzare il ComboBox DropDownStyle proprietà per modificare il comportamento del ComboBox. Questa proprietà accetta i valori possibili:This property accepts there possible values:

- DropDown - (valore predefinito) Il controllo ComboBox visualizza un elenco a discesa quando si fa clic sulla freccia ed è possibile immettere un valore personalizzato.
- Semplice - Il controllo ComboBox visualizza automaticamente un elenco a discesa ed è possibile immettere un valore personalizzato.
- DropDownList - Il controllo ComboBox funziona come un controllo DropDownList.

Il diverso tra DropDown e Simple è quando viene visualizzato l'elenco di elementi. Nel caso di Simple, l'elenco viene visualizzato immediatamente quando si sposta lo stato attivo sul controllo ComboBox. Nel caso di DropDown, è necessario fare clic sulla freccia per visualizzare l'elenco degli elementi.

Il valore DropDownList fa sì che il controllo ComboBox funzioni come un controllo DropDownList standard. Tuttavia, c'è una differenza importante qui. Nelle versioni precedenti di Internet Explorer viene visualizzato un controllo DropDownList con un indice z infinito in modo che il controllo venga visualizzato davanti a qualsiasi controllo posizionato davanti a esso. Poiché il controllo &lt;ComboBox&gt; esegue il &lt;rendering&gt; di un tag div HTML anziché di un tag di selezione HTML, il controllo ComboBox rispetta correttamente l'ordinamento z.

## <a name="setting-the-autocompletemode"></a>Impostazione di AutoCompleteMode

Utilizzare il ComboBox AutoCompleteMode proprietà per specificare cosa accade quando qualcuno digita testo nel comboBox. Questa proprietà accetta i seguenti valori possibili:

- Nessuno - (valore predefinito) Il controllo ComboBox non fornisce alcun comportamento di completamento automatico.
- Suggerimento - Il controllo ComboBox visualizza l'elenco ed evidenzia l'elemento corrispondente nell'elenco (vedere Figura 8).
- Append - Il controllo ComboBox non visualizza l'elenco e aggiunge l'elemento corrispondente dall'elenco a quello che è stato digitato (vedere la figura 9).
- SuggestAppend - Il comboBox visualizza l'elenco e aggiunge l'elemento corrispondente dall'elenco a quello che è stato digitato (vedere Figura 10).

[![Il ComboBox fa un suggerimento](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figura 08**: il controllo ComboBox crea un suggerimento([Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image16.png)a dimensioni intere )

[![ComboBox aggiunge il testo corrispondente](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figura 09**: ComboBox aggiunge il testo corrispondente([Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image18.png)a dimensioni intere )

[![Il comboBox suggerisce e aggiunge](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figura 10**: Il controllo ComboBox suggerisce e aggiunge([Fare clic per visualizzare l'immagine](how-do-i-use-the-combobox-control-vb/_static/image20.png)a dimensioni intere )

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come utilizzare il controllo ComboBox per visualizzare un set fisso di elementi. Il controllo ComboBox è stato associato sia a un set statico di elementi che a una tabella di database. Infine, è stato illustrato come modificare il comportamento del controllo ComboBox impostandone le proprietà DropDownStyle e AutoCompleteMode.

> [!div class="step-by-step"]
> [Indietro](how-do-i-use-the-combobox-control-cs.md)
