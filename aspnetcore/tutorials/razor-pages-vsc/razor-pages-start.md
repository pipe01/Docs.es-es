---
title: Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para compilar una aplicación web de páginas de Razor de ASP.NET Core con Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244858"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda leer [Introduction to Razor Pages](xref:razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial. Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

Desde un terminal, ejecute estos comandos:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Los comandos anteriores usan el [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor. Abra http://localhost:5000 en un explorador para ver la aplicación.

![Página Inicio o Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Abrir el proyecto

Presione CTRL+C para cerrar la aplicación.

En Visual Studio Code (VS Code), seleccione **Archivo > Abrir carpeta** y elija la carpeta *RazorPagesMovie*.

- Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).
- Seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".

### <a name="launch-the-app"></a>Iniciar la aplicación

En el menú **Depurar**, seleccione **Iniciar sin depurar**. Como alternativa, puede presionar el método abreviado de teclado que aparece junto a la opción de menú. Este método abreviado varía según el sistema operativo.

En el tutorial siguiente, agregaremos un modelo al proyecto. 

> [!div class="step-by-step"]
> [Siguiente: Adición de un modelo](xref:tutorials/razor-pages-vsc/model)  
