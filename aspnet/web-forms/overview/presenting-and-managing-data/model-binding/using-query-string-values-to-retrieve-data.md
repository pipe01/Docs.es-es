---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usar valores de cadena de consulta para filtrar los datos con el enlace de modelos y formularios web Forms | Documentos de Microsoft
author: tfitzmac
description: "Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="7081a-104">Uso de valores de cadena de consulta para filtrar los datos con el enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="7081a-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="7081a-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7081a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7081a-106">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7081a-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="7081a-107">Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="7081a-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="7081a-108">Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="7081a-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="7081a-109">Este tutorial muestra cómo pasar un valor en la cadena de consulta y use ese valor para recuperar los datos a través del enlace del modelo.</span><span class="sxs-lookup"><span data-stu-id="7081a-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="7081a-110">Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.</span><span class="sxs-lookup"><span data-stu-id="7081a-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="7081a-111">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="7081a-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="7081a-112">El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7081a-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="7081a-113">Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="7081a-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="7081a-114">Lo que vamos a compilar</span><span class="sxs-lookup"><span data-stu-id="7081a-114">What you'll build</span></span>

<span data-ttu-id="7081a-115">En este tutorial, deberá:</span><span class="sxs-lookup"><span data-stu-id="7081a-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="7081a-116">Agregue una nueva página para mostrar los cursos inscritos para un estudiante</span><span class="sxs-lookup"><span data-stu-id="7081a-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="7081a-117">Recuperar los cursos inscritos de los estudiantes seleccionado basándose en un valor de la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="7081a-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="7081a-118">Agregar un hipervínculo con un valor de cadena de consulta de la vista de cuadrícula a la nueva página</span><span class="sxs-lookup"><span data-stu-id="7081a-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="7081a-119">Los pasos de este tutorial son bastante similares a lo que hizo en el anterior [tutorial](sorting-paging-and-filtering-data.md) para filtrar los alumnos mostrados en función de la selección del usuario en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="7081a-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="7081a-120">En este tutorial, ha utilizado la **Control** atributo en el método select para especificar que el valor del parámetro procede de un control.</span><span class="sxs-lookup"><span data-stu-id="7081a-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="7081a-121">En este tutorial, usará el **QueryString** atributo en el método select para especificar que el valor del parámetro procede de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="7081a-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="7081a-122">Agregar nueva página para mostrar cursos de un alumno</span><span class="sxs-lookup"><span data-stu-id="7081a-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="7081a-123">Agregue un nuevo formulario web que usa la página maestra Site.master y el nombre de la página **cursos**.</span><span class="sxs-lookup"><span data-stu-id="7081a-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="7081a-124">En el **Courses.aspx** de archivos, agregar una vista de cuadrícula para mostrar los cursos del alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="7081a-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="7081a-125">Definir el método de selección</span><span class="sxs-lookup"><span data-stu-id="7081a-125">Define the select method</span></span>

<span data-ttu-id="7081a-126">En **Courses.aspx.cs**, agregará el método de selección con el nombre especificado en la vista de cuadrícula **SelectMethod** propiedad.</span><span class="sxs-lookup"><span data-stu-id="7081a-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="7081a-127">En ese método, deberá definir la consulta para recuperar los cursos de un alumno y especificar que el parámetro procede de un valor de cadena de consulta con el mismo nombre que el parámetro.</span><span class="sxs-lookup"><span data-stu-id="7081a-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="7081a-128">En primer lugar, debe agregar la siguiente **con** instrucciones.</span><span class="sxs-lookup"><span data-stu-id="7081a-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="7081a-129">A continuación, agregue el código siguiente a Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="7081a-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="7081a-130">El atributo de cadena de consulta significa que un valor de cadena de consulta denominado StudentID se asigna automáticamente al parámetro de este método.</span><span class="sxs-lookup"><span data-stu-id="7081a-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="7081a-131">Agregar hipervínculo con valor de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="7081a-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="7081a-132">En la vista de cuadrícula de Students.aspx, agregará un campo de hipervínculo que se vincula a la nueva página de cursos.</span><span class="sxs-lookup"><span data-stu-id="7081a-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="7081a-133">El hipervínculo incluirá un valor de cadena de consulta con el identificador del estudiante.</span><span class="sxs-lookup"><span data-stu-id="7081a-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="7081a-134">En Students.aspx, agregue el siguiente campo a las columnas de la vista de cuadrícula justo debajo del campo para créditos Total.</span><span class="sxs-lookup"><span data-stu-id="7081a-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="7081a-135">Ejecute la aplicación y observe que la vista de cuadrícula incluye ahora el vínculo cursos.</span><span class="sxs-lookup"><span data-stu-id="7081a-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Agregar hipervínculo](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="7081a-137">Al hacer clic en uno de los vínculos, podrá ver cursos inscritos de ese estudiante.</span><span class="sxs-lookup"><span data-stu-id="7081a-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="7081a-139">Conclusión</span><span class="sxs-lookup"><span data-stu-id="7081a-139">Conclusion</span></span>

<span data-ttu-id="7081a-140">En este tutorial, ha agregado un vínculo con un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="7081a-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="7081a-141">Usa ese valor de cadena de consulta para el valor del parámetro en el método select.</span><span class="sxs-lookup"><span data-stu-id="7081a-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="7081a-142">En la siguiente [tutorial](adding-business-logic-layer.md), se moverá el código de los archivos de código subyacente en una capa de lógica de negocios y una capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="7081a-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7081a-143">[Anterior](integrating-jquery-ui.md)
[Siguiente](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="7081a-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>