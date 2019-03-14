---
ms.openlocfilehash: 72e33ea44976963193d2560427fc418875be450e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038868"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="790fd-101">Aggiungere un modello a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="790fd-101">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="790fd-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="790fd-102">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="790fd-103">In questa sezione si aggiungeranno alcune classi per la gestione di film in un database.</span><span class="sxs-lookup"><span data-stu-id="790fd-103">In this section, you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="790fd-104">Queste classi saranno la parte "**M**odel" dell'app **M**VC.</span><span class="sxs-lookup"><span data-stu-id="790fd-104">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="790fd-105">Si usano queste classi con [Entity Framework Core](/ef/core) (EF Core) per usare un database.</span><span class="sxs-lookup"><span data-stu-id="790fd-105">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="790fd-106">EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="790fd-106">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span> <span data-ttu-id="790fd-107">[EF Core supporta molti motori di database](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="790fd-107">[EF Core supports many database engines](/ef/core/providers/).</span></span>

<span data-ttu-id="790fd-108">Le classi del modello che verranno create sono note come classi POCO (da "plain-old CLR Object") perché non contengono dipendenze da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="790fd-108">The model classes you'll create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="790fd-109">Definiscono semplicemente le proprietà dei dati che verranno archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="790fd-109">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="790fd-110">In questa esercitazione si scrivono innanzitutto le classi di modello ed Entity Framework Core creerà il database.</span><span class="sxs-lookup"><span data-stu-id="790fd-110">In this tutorial you'll write the model classes first, and EF Core will create the database.</span></span> <span data-ttu-id="790fd-111">Un approccio alternativo non trattato in questo articolo consiste nel generare classi del modello da un database già esistente.</span><span class="sxs-lookup"><span data-stu-id="790fd-111">An alternate approach not covered here is to generate model classes from an already-existing database.</span></span> <span data-ttu-id="790fd-112">Per informazioni su questo approccio, vedere [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (Introduzione a EF Core su ASP.NET Core con un database esistente).</span><span class="sxs-lookup"><span data-stu-id="790fd-112">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="790fd-113">Aggiungere una classe del modello di dati</span><span class="sxs-lookup"><span data-stu-id="790fd-113">Add a data model class</span></span>
