---
title: Metapacchetto Microsoft.AspNetCore.App per ASP.NET Core 2.1 e versioni successive
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.App include tutti i pacchetti ASP.NET Core e Entity Framework Core supportati.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 68b5aca60273a8c6ef03c0a29842e6a5305adeb3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034438"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Metapacchetto Microsoft.AspNetCore.App per ASP.NET Core 2.1

Questa funzionalità richiede ASP.NET Core 2.1 e versioni successive con destinazione .NET Core 2.1 e versioni successive.

Il [metapacchetto](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [Microsoft.AspNetCore.App](/dotnet/core/packages#metapackages) per ASP.NET Core:

* Non include le dipendenze di terze parti, ad eccezione di [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) e [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Queste dipendenze di terze parti sono necessarie per garantire il funzionamento delle principali funzionalità dei framework.
* Include tutti i pacchetti supportati dal team di ASP.NET Core ad eccezione di quelli che contengono dipendenze di terze parti (diverse da quelle indicate in precedenza).
* Include tutti i pacchetti supportati dal team di Entity Framework Core ad eccezione di quelli che contengono dipendenze di terze parti (diverse da quelle indicate in precedenza).

Tutte le funzionalità di ASP.NET Core 2.1 e versioni successive e Entity Framework Core 2.1 e versioni successive sono incluse nel pacchetto `Microsoft.AspNetCore.App`. I modelli di progetto predefiniti destinati ad ASP.NET Core 2.1 e versioni successive usano questo pacchetto. È consigliabile che le applicazioni con destinazione ASP.NET Core 2.1 e versioni successive e Entity Framework Core 2.1 e versioni successive usino il pacchetto `Microsoft.AspNetCore.App`.

Il numero di versione del metapacchetto `Microsoft.AspNetCore.App` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core.

L'uso del metapacchetto `Microsoft.AspNetCore.App` implica restrizioni di versione che proteggono l'app:

* Se viene incluso un pacchetto che presenta una dipendenza transitiva (non diretta) in un pacchetto in `Microsoft.AspNetCore.App` e i numeri di versione sono diversi, NuGet genererà un errore.
* Altri pacchetti aggiunti all'app non possono modificare la versione dei pacchetti inclusi in `Microsoft.AspNetCore.App`.
* La coerenza della versione garantisce un'esperienza affidabile. `Microsoft.AspNetCore.App` è stato progettato per impedire le combinazioni di versioni non testate di bit correlati usati contemporaneamente nella stessa applicazione.

Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.App` sfruttano automaticamente il framework condiviso di ASP.NET Core. Quando si usa il metapacchetto `Microsoft.AspNetCore.App`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nel framework condiviso di .NET Core. Gli asset contenuti nel framework condiviso sono precompilati per migliorare i tempi di avvio dell'applicazione. Per altre informazioni, vedere la sezione relativa ai pacchetti condivisi in [Pacchetti di distribuzione di .NET Core](/dotnet/core/build/distribution-packaging).

Il file di progetto seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.App` per ASP.NET Core e rappresenta un modello ASP.NET Core 2.1 tipico:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

Il markup precedente rappresenta un modello tipico di ASP.NET Core 2.1 e versioni successive. Non specifica un numero di versione per il pacchetto di riferimento `Microsoft.AspNetCore.App`. Quando la versione non è specificata, una versione [implicita](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) viene specificata dall'SDK, vale a dire `Microsoft.NET.Sdk.Web`. È consigliabile basarsi sulla versione implicita specificata dall'SDK e non impostando in modo esplicito il numero di versione sul riferimento al pacchetto. In caso di domande su questo approccio, è possibile lasciare un commento GitHub nella pagina della [discussione per la versione implicita Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).

La versione implicita è impostata su `major.minor.0` per le app portabili. Il meccanismo di roll forward del framework condiviso eseguirà l'app sulla versione compatibile più recente tra i framework condivisi installati. Per garantire che venga usata la stessa versione in fase di sviluppo, test e produzione, verificare che in tutti gli ambienti sia installata la stessa versione del framework condiviso. Per le app autonome, il numero di versione implicita è impostato sul valore `major.minor.patch` del framework condiviso nel bundle nell'SDK installato.

Specificando un numero di versione nel riferimento `Microsoft.AspNetCore.App` **non** si garantisce che verrà scelta la versione del framework condiviso. Si supponga ad esempio che venga specificata la versione "2.1.1" ma che sia installata la versione "2.1.3". In tal caso, l'app userà "2.1.3". Benché non sia consigliabile, è possibile disabilitare il roll forward (patch e/o versioni secondarie). Per altre informazioni su come eseguire il roll forward dell'host dotnet e configurarne il comportamento, vedere [dotnet host rollforward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (Roll forward dell'host dotnet).

`<Project Sdk` deve essere impostato su `Microsoft.NET.Sdk.Web` per usare la versione implicita `Microsoft.AspNetCore.App`.  Quando si usa `<Project Sdk="Microsoft.NET.Sdk">` (senza il carattere finale `.Web`):

* Viene generato l'avviso seguente:

     *Avviso NU1604: Dipendenza del progetto Microsoft.AspNetCore.App non contiene un limite inferiore inclusivo. Includere un limite inferiore nella versione della dipendenza per garantire risultati di ripristino coerenti.*
* Si tratta di un problema noto con .NET Core 2.1 SDK, che verrà risolto in .NET Core 2.2 SDK.

<a name="update"></a>

## <a name="update-aspnet-core"></a>Aggiornare ASP.NET Core

Il [metapacchetto](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` non è un pacchetto tradizionale aggiornato da NuGet. Analogamente a `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` rappresenta un runtime condiviso, con una semantica di controllo delle versioni speciale gestita all'esterno di NuGet. Per altre informazioni, vedere [Pacchetti, metapacchetti e framework](/dotnet/core/packages).

Per aggiornare ASP.NET Core:

* Nel computer di sviluppo e i server di compilazione: Scaricare e installare il [.NET Core SDK](https://www.microsoft.com/net/download).
* Nel server di distribuzione: Scaricare e installare il [runtime di .NET Core](https://www.microsoft.com/net/download).

 Le applicazioni eseguiranno il roll forward all'ultima versione installata al riavvio dell'applicazione. Non è necessario aggiornare il numero di versione di `Microsoft.AspNetCore.App` nel file di progetto. Per altre informazioni, vedere [Roll forward delle app dipendenti dal framework](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Se l'applicazione usava in precedenza `Microsoft.AspNetCore.All`, vedere [Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
