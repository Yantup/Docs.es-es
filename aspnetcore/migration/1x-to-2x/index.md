---
title: "Migración de ASP.NET Core 1.x a 2.0"
author: scottaddie
description: "En este artículo se indican los requisitos previos y los pasos más comunes para migrar un proyecto de ASP.NET Core 1.x a ASP.NET Core 2.0."
keywords: "ASP.NET Core,migración"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: c14e7d61e8b353c18fc4a4f2bf3658069982bad5
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>Migración de ASP.NET Core 1.x a ASP.NET Core 2.0

Por [Scott Addie](https://github.com/scottaddie)

En este artículo se le guía a lo largo del proceso de actualización de un proyecto existente de ASP.NET Core 1.x a ASP.NET Core 2.0. La migración de la aplicación a ASP.NET Core 2.0 permite aprovechar [muchas características nuevas y mejoras de rendimiento](https://go.microsoft.com/fwlink/?linkid=854094). 

Las aplicaciones existentes de ASP.NET Core 1.x se basan en plantillas de proyecto específicas de la versión. A medida que el marco de trabajo de ASP.NET Core evoluciona, también lo hacen las plantillas de proyecto y el código de inicio incluido en ellas. Además de actualizar el marco de trabajo de ASP.NET Core, debe actualizar el código de la aplicación.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos
Vea [Introducción a ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Actualización del moniker de la plataforma de destino (TFM)
Los proyectos para .NET Core deben usar el [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de una versión mayor o igual que .NET Core 2.0. Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `netcoreapp2.0`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]

Los proyectos para .NET Framework deben usar el TFM de una versión mayor o igual que .NET Framework 4.6.1. Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `net461`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET Core 2.0 ofrece un área expuesta mucho mayor que .NET Core 1.x. Si el destino es .NET Framework solo porque faltan API en .NET Core 1.x, el uso de .NET Core 2.0 como destino es probable que dé resultado.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Actualización de la versión del SDK de .NET Core en global.json
Si la solución se basa en un archivo [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) para que el destino sea una versión específica del SDK de .NET Core, actualice su propiedad `version` para usar la versión 2.0 instalada en el equipo:

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Actualización de las referencias del paquete
El archivo *.csproj* de un proyecto de 1.x enumera cada paquete NuGet usado por el proyecto.

En un proyecto de ASP.NET Core 2.0 para .NET Core 2.0, una sola referencia de [metapaquete](xref:fundamentals/metapackage) del archivo *.csproj* reemplaza a la colección de paquetes:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]

Todas las características de ASP.NET Core 2.0 y Entity Framework Core 2.0 están incluidas en el metapaquete.

Los proyectos de ASP.NET Core 2.0 para .NET Framework deben seguir haciendo referencia a paquetes NuGet individuales. Actualización del atributo `Version` de cada nodo `<PackageReference />` a 2.0.0.

Por ejemplo, esta es la lista de nodos `<PackageReference />` usados en un proyecto típico de ASP.NET Core 2.0 para .NET Framework:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Actualización de las herramientas de la CLI de .NET Core
En el archivo *.csproj*, actualice el atributo `Version` de cada nodo `<DotNetCliToolReference />` a 2.0.0.

Por ejemplo, esta es la lista de herramientas de la CLI usadas en un proyecto típico de ASP.NET Core 2.0 para .NET Core 2.0:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Cambio de nombre de la propiedad Package Target Fallback
El archivo *.csproj* de un proyecto de 1.x usa un nodo `PackageTargetFallback` y una variable:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]

Cambie el nombre del nodo y la variable a `AssetTargetFallback`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Actualización del método Main de Program.cs
En los proyectos de 1.x, el método `Main` de *Program.cs* tenía este aspecto:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

En los proyectos de 2.0, el método `Main` de *Program.cs* se ha simplificado:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]

La adopción de este nuevo patrón 2.0 es muy recomendable y necesaria para que funcionen características de producto como las [migraciones de Entity Framework Core](xref:data/ef-mvc/migrations). Por ejemplo, la ejecución de `Update-Database` desde la ventana Consola del Administrador de paquetes o de `dotnet ef database update` desde la línea de comandos (en proyectos convertidos a ASP.NET Core 2.0) genera el error siguiente:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a>Revisión de la configuración de compilación de la vista Razor
Un tiempo de inicio de aplicación más rápido y unos lotes publicados más pequeños son de la máxima importancia para el usuario. Por ello, la [compilación de la vista Razor](xref:mvc/views/view-compilation) está habilitada de forma predeterminada en ASP.NET Core 2.0.

Ya no es necesario establecer la propiedad `MvcRazorCompileOnPublish` en true. A menos que se esté deshabilitando la compilación de la vista, se puede quitar la propiedad del archivo *.csproj*.

Cuando el destino es .NET Framework, se debe hacer referencia de forma explícita al paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) en el archivo *.csproj*:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Características "Light-Up" de Application Insights como base
Es importante configurar sin esfuerzo la instrumentación de rendimiento de la aplicación. Ahora puede basarse en las nuevas características "light-up" de [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponibles en las herramientas de Visual Studio 2017.

Los proyectos de ASP.NET Core 1.1 creados en Visual Studio 2017 han agregado Application Insights de forma predeterminada. Si no usa el SDK de Application Insights directamente, fuera de *Program.cs* y *Startup.cs*, siga estos pasos:

1. Quite el siguiente nodo `<PackageReference />` del archivo *.csproj*:
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]

2. Quite la invocación del método de extensión `UseApplicationInsights` de *Program.cs*:

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Quite la llamada API del lado cliente de Application Insights de *_Layout.cshtml*. Comprende las dos líneas de código siguientes:

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]

Si usa el SDK de Application Insights directamente, siga haciéndolo. El [metapaquete](xref:fundamentals/metapackage) 2.0 incluye la versión más reciente de Application Insights, por lo que si se hace referencia a una versión anterior, aparece un error de degradación de paquete.

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a>Adopción de mejoras de autenticación o Identity
ASP.NET Core 2.0 tiene un nuevo modelo de autenticación y una serie de cambios significativos en ASP.NET Core Identity. Si ha creado el proyecto con la opción Cuentas de usuario individuales habilitada o si ha agregado manualmente la autenticación o Identity, vea [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x) (Migración de la autenticación e Identity a ASP.NET Core 2.0).

## <a name="additional-resources"></a>Recursos adicionales
- [Cambios importantes en ASP.NET Core 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)