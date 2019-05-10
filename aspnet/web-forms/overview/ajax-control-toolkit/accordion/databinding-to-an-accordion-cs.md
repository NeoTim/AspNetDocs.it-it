---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Data Binding a un controllo Accordion (c#) | Microsoft Docs
author: wenz
description: Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono dichiarati in genere w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: d908f89ea1a2b91b9dd7a26d72160e9f38e69c29
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65133692"
---
# <a name="databinding-to-an-accordion-c"></a>Data binding a un controllo Accordion (C#)

da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli in genere vengono dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.

## <a name="overview"></a>Panoramica

Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli in genere vengono dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.

## <a name="steps"></a>Passaggi

Prima di tutto, è richiesta un'origine dati. Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati alla pagina. Per usare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si usa l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente non prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Ricordare il nome (ID) dell'origine dati. Questa identificazione molto deve quindi essere usata nel `DataSourceID` proprietà del controllo Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

All'interno del controllo Accordion, è possibile fornire modelli per varie parti del controllo, compresa l'intestazione (`<HeaderTemplate>`) e il contenuto (`<ContentTemplate>`). All'interno di questi elementi, restituire solo i dati dai dati di origine, utilizzando il `DataBinder.Eval()` metodo:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Quando la pagina viene caricata, l'origine dati deve essere associato ai accordion con questo codice lato server:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Per concludere questo esempio, è necessario definire le due classi CSS a cui fanno riferimento nel controllo Accordion (nelle relative proprietà `HeaderCssClass` e `ContentCssClass`). Inserire il seguente markup nel `<head>` sezione della pagina:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

[![I dati di controllo accordion provengono direttamente dall'origine dati](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

I dati di controllo accordion provengono direttamente dall'origine dati ([fare clic per visualizzare l'immagine con dimensioni normali](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](dynamically-adding-an-accordion-pane-cs.md)
