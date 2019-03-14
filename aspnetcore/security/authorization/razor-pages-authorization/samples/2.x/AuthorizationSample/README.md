---
ms.openlocfilehash: 6ceb4bd6580d11ee5d6ff57a895c499308035858
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064038"
---
# <a name="aspnet-core-authorization-sample"></a><span data-ttu-id="11f29-101">Esempio di autorizzazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11f29-101">ASP.NET Core Authorization Sample</span></span>

<span data-ttu-id="11f29-102">In questo esempio viene illustrato l'utilizzo di autorizzazione di Razor Pages dalle convenzioni.</span><span class="sxs-lookup"><span data-stu-id="11f29-102">This sample illustrates use of Razor Pages authorization by conventions.</span></span> <span data-ttu-id="11f29-103">In questo esempio illustra le funzionalità descritte nel [convenzioni di autorizzazione di Razor Pages](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) argomento.</span><span class="sxs-lookup"><span data-stu-id="11f29-103">This sample demonstrates the features described in the [Razor Pages authorization conventions](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) topic.</span></span>

<span data-ttu-id="11f29-104">Autorizzazione utente in questo esempio Usa l'autenticazione dei cookie funzionalità descritte nel [Usa autenticazione di cookie senza ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) argomento.</span><span class="sxs-lookup"><span data-stu-id="11f29-104">User authorization in this sample uses the cookie authentication features described in the [Use cookie authentication without ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) topic.</span></span> <span data-ttu-id="11f29-105">Per informazioni sull'uso di ASP.NET Core Identity, vedere <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="11f29-105">For information on using ASP.NET Core Identity, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="11f29-106">Quando si esegue l'esempio, usare l'indirizzo di posta elettronica **maria.rodriguez@contoso.com** per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="11f29-106">When running the sample, use the email address **maria.rodriguez@contoso.com** to authenticate the user.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="11f29-107">Esempi</span><span class="sxs-lookup"><span data-stu-id="11f29-107">Examples in this sample</span></span>

| <span data-ttu-id="11f29-108">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="11f29-108">Feature</span></span> | <span data-ttu-id="11f29-109">Descrizione</span><span class="sxs-lookup"><span data-stu-id="11f29-109">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="11f29-110">AuthorizePage</span><span class="sxs-lookup"><span data-stu-id="11f29-110">AuthorizePage</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | <span data-ttu-id="11f29-111">Aggiunge un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina con il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="11f29-111">Adds an [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page with the specified path.</span></span> |
| [<span data-ttu-id="11f29-112">AuthorizeFolder</span><span class="sxs-lookup"><span data-stu-id="11f29-112">AuthorizeFolder</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | <span data-ttu-id="11f29-113">Aggiunge un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) per tutte le pagine in una cartella con il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="11f29-113">Adds an [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder with the specified path.</span></span> |
| [<span data-ttu-id="11f29-114">AllowAnonymousToPage</span><span class="sxs-lookup"><span data-stu-id="11f29-114">AllowAnonymousToPage</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | <span data-ttu-id="11f29-115">Aggiunge un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina con il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="11f29-115">Adds an [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page with the specified path.</span></span> |
| [<span data-ttu-id="11f29-116">AllowAnonymousToFolder</span><span class="sxs-lookup"><span data-stu-id="11f29-116">AllowAnonymousToFolder</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | <span data-ttu-id="11f29-117">Aggiunge un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) per tutte le pagine in una cartella con il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="11f29-117">Adds an [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder with the specified path.</span></span> |