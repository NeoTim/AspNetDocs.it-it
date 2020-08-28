---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Creazione di una stringa di connessione e utilizzo di SQL Server database locale | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 4400cb8c6a5d57da60d164220f929d69d7f4d2f7
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044259"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e uso di SQL Server LocalDB

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e uso di SQL Server LocalDB

La `MovieDBContext` classe creata gestisce l'attività di connessione al database e di mapping `Movie` degli oggetti ai record del database. Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà. In realtà, non è necessario specificare il database da utilizzare, Entity Framework utilizzerà per impostazione predefinita il database [locale](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). In questa sezione verrà aggiunta in modo esplicito una stringa di connessione nel file di *Web.config* dell'applicazione.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Il [database locale](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) è una versione leggera del SQL Server Express motore di database che viene avviato su richiesta e viene eseguito in modalità utente. Il database locale viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di usare i database come file con *estensione MDF* . In genere, i file di database del database locale vengono conservati nella cartella dei * \_ dati dell'app* di un progetto Web.

Non è consigliabile usare SQL Server Express per le applicazioni Web di produzione. In particolare, il database locale non deve essere usato per la produzione con un'applicazione Web perché non è progettato per funzionare con IIS. È tuttavia possibile eseguire facilmente la migrazione di un database del database locale a SQL Server o SQL Azure.

In Visual Studio 2017, il database locale viene installato per impostazione predefinita con Visual Studio.

Per impostazione predefinita, il Entity Framework cerca una stringa di connessione denominata uguale alla classe del contesto dell'oggetto ( `MovieDBContext` per questo progetto). Per ulteriori informazioni, vedere [SQL Server stringhe di connessione per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Aprire il file radice dell'applicazione *Web.config* mostrato di seguito. (Non il file *Web.config* nella cartella *views* ).

![](creating-a-connection-string/_static/image1.png)

Trovare l' `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Aggiungere la stringa di connessione seguente all' `<connectionStrings>` elemento nel file di *Web.config* .

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Nell'esempio seguente viene illustrata una parte del file *Web.config* con la nuova stringa di connessione aggiunta:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Le due stringhe di connessione sono molto simili. La prima stringa di connessione è denominata `DefaultConnection` e viene utilizzata per il database delle appartenenze per controllare chi può accedere all'applicazione. La stringa di connessione aggiunta specifica un database local DB denominato *Movie. MDF* presente nella cartella * \_ dei dati dell'app* . In questa esercitazione non verrà usato il database delle appartenenze. per altre informazioni sull'appartenenza, l'autenticazione e la sicurezza, vedere l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Il nome della stringa di connessione deve corrispondere al nome della classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

In realtà non è necessario aggiungere la `MovieDBContext` stringa di connessione. Se non si specifica una stringa di connessione, Entity Framework creerà un database del database locale nella directory Users con il nome completo della classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (in questo caso `MvcMovie.Models.MovieDBContext` ). È possibile assegnare un nome qualsiasi al database, purché sia presente *. * Suffisso MDF. Ad esempio, è possibile denominare il database *Films. MDF*.

Successivamente, verrà compilata una nuova `MoviesController` classe che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.

> [!div class="step-by-step"]
> [Precedente](adding-a-model.md) 
>  [Avanti](accessing-your-models-data-from-a-controller.md)
