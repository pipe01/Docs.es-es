---
title: Agregar búsqueda a páginas de Razor de ASP.NET Core
author: rick-anderson
description: Muestra cómo agregar la búsqueda a páginas de Razor de ASP.NET Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 80292f8cfecd5363fb8acc8578f9bb0ca9ee5969
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090168"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>Agregar búsqueda a páginas de Razor de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este documento se agrega a la página de índice una capacidad de búsqueda que permite buscar películas por *género* o *nombre*.

Agregue las siguientes propiedades resaltadas a *Pages/Movies/Index.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

* `SearchString`: contiene el texto que los usuarios escriben en el cuadro de texto de búsqueda.
* `Genres`: contiene la lista de géneros. Esto permite al usuario seleccionar un género de la lista.
* `MovieGenre`: contiene el género concreto que selecciona el usuario (por ejemplo, "Western").

Trabajará con las propiedades `Genres` y `MovieGenre` más adelante en este documento.

Actualice el método `OnGetAsync` de la página de índice con el código siguiente:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

La primera línea del método `OnGetAsync` crea una consulta [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) para seleccionar las películas:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

En este momento *solo* se define la consulta, **no** se ejecuta en la base de datos.

Si el parámetro `searchString` contiene una cadena, la consulta de películas se modifica para filtrar según la cadena de búsqueda:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

El código `s => s.Title.Contains()` es una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Las lambdas se usan en consultas [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basadas en métodos como argumentos para métodos de operador de consulta estándar, como el método [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) o `Contains` (usado en el código anterior). Las consultas LINQ no se ejecutan cuando se definen ni cuando se modifican mediante una llamada a un método (como `Where`, `Contains` u `OrderBy`). En su lugar, se aplaza la ejecución de la consulta. Esto significa que la evaluación de una expresión se aplaza hasta que su valor realizado se repite o se llama al método `ToListAsync`. Para más información, vea [Query Execution (Ejecución de consultas)](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

**Nota:** El método [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) se ejecuta en la base de datos, no en el código de C#. La distinción entre mayúsculas y minúsculas en la consulta depende de la base de datos y la intercalación. En SQL Server, `Contains` se asigna a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), que distingue entre mayúsculas y minúsculas. En SQLite, con la intercalación predeterminada, se distingue entre mayúsculas y minúsculas.

Por último, la línea final del método `OnGetAsync` rellena la propiedad `SearchString` con el valor de la búsqueda del usuario. Con la propiedad `SearchString` rellenada, el valor de búsqueda se conserva en el cuadro de búsqueda después de ejecutarla.

Vaya a la página de películas y anexe una cadena de consulta como `?searchString=Ghost` a la dirección URL (por ejemplo, `http://localhost:5000/Movies?searchString=Ghost`). Se muestran las películas filtradas.

![Vista de índice](search/_static/ghost.png)

Si se agrega la siguiente plantilla de ruta a la página de índice, la cadena de búsqueda se puede pasar como un segmento de dirección URL (por ejemplo, `http://localhost:5000/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

La restricción de ruta anterior permite buscar el título como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.  El elemento `?` de `"{searchString?}"` significa que se trata de un parámetro de ruta opcional.

![Vista de índice con la palabra Ghost (Fantasma) agregada a la dirección URL y una lista de películas devuelta con dos películas, Ghostbusters y Ghostbusters 2](search/_static/g2.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL para buscar una película. En este paso, se agrega la interfaz de usuario para filtrar las películas. Si ha agregado la restricción de ruta `"{searchString?}"`, quítela.

Abra el archivo *Pages/Movies/Index.cshtml* y agregue el marcado `<form>` resaltado en el siguiente código:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

La etiqueta HTML `<form>` usa el [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper). Cuando se envía el formulario, la cadena de filtro se envía a la página *Pages/Movies/Index*. Guarde los cambios y pruebe el filtro.

![Vista de índice con la palabra Ghost (Fantasma) escrita en el cuadro de texto del filtro de título](search/_static/filter.png)

## <a name="search-by-genre"></a>Búsqueda por género

Actualice el método `OnGetAsync` con el código siguiente:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

La `SelectList` de géneros se crea mediante la proyección de los distintos géneros.

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a>Adición de búsqueda por género

Actualice *Index.cshtml* como se indica a continuación:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Pruebe la aplicación al buscar por género, título de la película y ambos.

> [!div class="step-by-step"]
> [Anterior: Actualización de páginas](xref:tutorials/razor-pages/da1)
> [Siguiente: Adición de un nuevo campo](xref:tutorials/razor-pages/new-field)
