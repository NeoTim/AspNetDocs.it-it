---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Creazione di un'azione (VB) Documenti Microsoft
author: rick-anderson
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo come azione.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: dd651155bdb931cb8358d369b3c0c2c98abb86b2
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542248"
---
# <a name="creating-an-action-vb"></a>Creazione di un'azione (VB)

da parte [di Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo come azione.

L'obiettivo di questa esercitazione è spiegare come è possibile creare una nuova azione del controller. Informazioni sui requisiti di un metodo di azione. Si apprenderà inoltre come impedire l'esposizione di un metodo come azione.

## <a name="adding-an-action-to-a-controller"></a>Aggiunta di un'azione a un controllerAdding an Action to a Controller

Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller. Ad esempio, il controller nel listato 1 contiene un'azione denominata Index() e un'azione denominata SayHello(). Entrambi i metodi vengono esposti come azioni.

**Listato 1 - Controllers-HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Per essere esposto all'universo come azione, un metodo deve soddisfare determinati requisiti:

- Il metodo deve essere pubblico.
- Il metodo non può essere un metodo statico.
- Il metodo non può essere un metodo di estensione.
- Il metodo non può essere un costruttore, getter o setter.
- Il metodo non può avere tipi generici aperti.
- Il metodo non è un metodo della classe base del controller.
- Il metodo non può contenere parametri **ref** o **out.**

Si noti che non esistono restrizioni sul tipo restituito di un'azione del controller. Un'azione del controller può restituire una stringa, un DateTime, un'istanza della classe Random o void. Il framework di MVC ASP.NET convertirà qualsiasi tipo restituito che non è un risultato di azione in una stringa ed eseguirà il rendering della stringa nel browser.

Quando si aggiunge un metodo che non viola questi requisiti a un controller, il metodo viene esposto come azione del controller. Fate attenzione qui. A controller action can be invoked by anyone connected to the Internet. Non creare, ad esempio, un'azione del controller DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedire che un metodo pubblico venga richiamatoPreventing a Public Method From Being Invoked

Se è necessario creare un metodo pubblico in una classe controller e non si desidera esporre il metodo come azione &lt;del&gt; controller, è possibile impedire che il metodo venga richiamato utilizzando l'attributo NonAction.If you need to create a public method in a controller class and you don't want to expose the method as a controller action, you can prevent the method from being invoked by using the NonAction attribute. Ad esempio, il controller nel listato 2 contiene un metodo &lt;pubblico&gt; denominato CompanySecrets() decorato con l'attributo NonAction.

**Listato 2 - Controllers-WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Se si tenta di richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser, verrà visualizzato il messaggio di errore nella Figura 1.

[![Richiamo di un metodo NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: richiamare un metodo NonAction[(Fare clic per visualizzare l'immagine a dimensioni intere)](creating-an-action-vb/_static/image2.png)

> [!div class="step-by-step"]
> [Successivo](creating-a-controller-vb.md)
> [precedente](aspnet-mvc-controllers-overview-cs.md)
