---
title: Eseguire la migrazione da Logging 3.0 o 2.1 a 2.2
author: pakrym
description: Informazioni su come eseguire la migrazione di un'applicazione non ASP.NET Core che usa Logging da 2.1 a 2.2 o 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063178"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Eseguire la migrazione da Logging 3.0 o 2.1 a 2.2

Questo articolo illustra i passaggi comuni per la migrazione di un'applicazione non ASP.NET Core che usa `Microsoft.Extensions.Logging` da 2.1 a 2.2 o 3.0.

## <a name="21-to-22"></a>Da 2.1 a 2.2

Creare manualmente `ServiceCollection` e chiamare `AddLogging`.

2.1 esempio:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

esempio 2.2:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 a 3.0

In 3.0, usare `LoggingFactory.Create`.

2.1 esempio:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

esempio 3.0:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Risorse aggiuntive

<xref:fundamentals/logging/index>