---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de la base de datos - 9 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 44faca6ac2cdb9193f5c7f6b1d7f5a26e6e22248
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="281e3-103">Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de la base de datos - 9 de 12</span><span class="sxs-lookup"><span data-stu-id="281e3-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="281e3-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="281e3-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="281e3-105">Descargar proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="281e3-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="281e3-106">Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web.</span><span class="sxs-lookup"><span data-stu-id="281e3-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="281e3-107">También puede usar Visual Studio 2010 si instala la actualización de publicación de Web.</span><span class="sxs-lookup"><span data-stu-id="281e3-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="281e3-108">Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="281e3-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="281e3-109">Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="281e3-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="281e3-110">Información general</span><span class="sxs-lookup"><span data-stu-id="281e3-110">Overview</span></span>

<span data-ttu-id="281e3-111">En este tutorial, realice un cambio de base de datos y los cambios de código relacionado, comprobar los cambios en Visual Studio, y luego implementar la actualización en entornos de prueba y producción.</span><span class="sxs-lookup"><span data-stu-id="281e3-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="281e3-112">Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="281e3-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="281e3-113">Agregar una nueva columna a una tabla</span><span class="sxs-lookup"><span data-stu-id="281e3-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="281e3-114">En esta sección, agregará una columna de fecha de nacimiento a la `Person` la clase base para la `Student` y `Instructor` entidades.</span><span class="sxs-lookup"><span data-stu-id="281e3-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="281e3-115">A continuación, actualice la página que muestra los datos de instructor para que se muestre la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="281e3-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="281e3-116">En el *ContosoUniversity.DAL* proyecto abierto *Person.cs* y agregue la siguiente propiedad al final de la `Person` clase (debe haber dos sigue llaves de cierre):</span><span class="sxs-lookup"><span data-stu-id="281e3-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="281e3-117">A continuación, actualizar el método de inicialización para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="281e3-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="281e3-118">Abra *Migrations\Configuration.cs* y reemplace el bloque de código que se inicia `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye información de fecha de nacimiento:</span><span class="sxs-lookup"><span data-stu-id="281e3-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="281e3-119">En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregar un nuevo campo de plantilla para mostrar la fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="281e3-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="281e3-120">Agregarlo entre los de asignación de fecha y la oficina de contratación:</span><span class="sxs-lookup"><span data-stu-id="281e3-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="281e3-121">(Si la sangría del código obtiene la sincronización, puede presionar CTRL-K y, a continuación, CTRL+D para volver a formatear automáticamente el archivo.)</span><span class="sxs-lookup"><span data-stu-id="281e3-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="281e3-122">Compile la solución y, a continuación, abra el **Package Manager Console** ventana.</span><span class="sxs-lookup"><span data-stu-id="281e3-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="281e3-123">Asegúrese de que ContosoUniversity.DAL todavía está seleccionado como el **proyecto predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="281e3-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="281e3-124">En el **Package Manager Console** ventana, seleccione **ContosoUniversity.DAL** como el **proyecto predeterminado**y, a continuación, escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="281e3-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="281e3-125">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` (clase) y en el `Up` método puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="281e3-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="281e3-127">Compile la solución y, a continuación, escriba el siguiente comando en el **Package Manager Console** ventana (asegúrese de que todavía esté seleccionado el proyecto de ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="281e3-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="281e3-128">Cuando finaliza el comando, ejecute la aplicación y seleccione la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="281e3-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="281e3-129">Cuando se carga la página, verá que tiene el nuevo campo de fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="281e3-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="281e3-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="281e3-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="281e3-131">Implementación de la actualización de la base de datos en el entorno de prueba</span><span class="sxs-lookup"><span data-stu-id="281e3-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="281e3-132">En **el Explorador de soluciones** seleccione el proyecto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="281e3-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="281e3-133">En el **Web uno haga clic en publicar** barra de herramientas, seleccione la **prueba** perfil de publicación y, a continuación, haga clic en **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="281e3-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="281e3-134">(Si se deshabilita la barra de herramientas, seleccione el proyecto ContosoUniversity en **el Explorador de soluciones**.)</span><span class="sxs-lookup"><span data-stu-id="281e3-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="281e3-135">Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal.</span><span class="sxs-lookup"><span data-stu-id="281e3-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="281e3-136">Ejecute la página de instructores para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="281e3-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="281e3-137">Cuando la aplicación intenta tener acceso a la base de datos de esta página, Code First actualiza el esquema de base de datos y se ejecuta el `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="281e3-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="281e3-138">Cuando se muestra la página, verá el esperado **la fecha de nacimiento** columna con fechas en ella.</span><span class="sxs-lookup"><span data-stu-id="281e3-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="281e3-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="281e3-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="281e3-140">Implementación de la actualización de la base de datos en el entorno de producción</span><span class="sxs-lookup"><span data-stu-id="281e3-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="281e3-141">Ahora puede implementar en producción.</span><span class="sxs-lookup"><span data-stu-id="281e3-141">You can now deploy to production.</span></span> <span data-ttu-id="281e3-142">La única diferencia es que podrá usar *aplicación\_offline.htm* para impedir que los usuarios tienen acceso al sitio y lo que actualiza la base de datos mientras se va a implementar cambios.</span><span class="sxs-lookup"><span data-stu-id="281e3-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="281e3-143">Para la implementación de producción, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="281e3-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="281e3-144">Cargar la *aplicación\_offline.htm* archivo para el sitio de producción.</span><span class="sxs-lookup"><span data-stu-id="281e3-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="281e3-145">En Visual Studio, elija el perfil de producción en el **Web uno haga clic en publicar** barra de herramientas y haga clic en **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="281e3-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="281e3-146">Eliminar el *aplicación\_offline.htm* archivo desde el sitio de producción.</span><span class="sxs-lookup"><span data-stu-id="281e3-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="281e3-147">Mientras la aplicación está en uso en el entorno de producción debe implementar un plan de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="281e3-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="281e3-148">Es decir, debe copiar periódicamente el *School-Prod.sdf* y *Prod.sdf aspnet* archivos de producción de sitio a una ubicación de almacenamiento seguro y debe mantener varias generaciones de estos copias de seguridad.</span><span class="sxs-lookup"><span data-stu-id="281e3-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="281e3-149">Cuando se actualiza la base de datos, debe realizar una copia de seguridad de inmediatamente antes del cambio.</span><span class="sxs-lookup"><span data-stu-id="281e3-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="281e3-150">A continuación, si comete un error y no detectar hasta después de haber implementado en producción, aún podrá recuperar la base de datos al estado que tenía antes de resultó dañado.</span><span class="sxs-lookup"><span data-stu-id="281e3-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="281e3-151">Cuando Visual Studio abre la URL de la página de inicio en el explorador, el *aplicación\_offline.htm* se muestra la página.</span><span class="sxs-lookup"><span data-stu-id="281e3-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="281e3-152">Después de eliminar el *aplicación\_offline.htm* archivo, que puede ir a la página principal nuevo para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="281e3-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="281e3-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="281e3-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="281e3-154">Ahora que ha implementado una actualización de la aplicación que incluye un cambio de base de datos en la prueba y producción.</span><span class="sxs-lookup"><span data-stu-id="281e3-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="281e3-155">El siguiente tutorial muestra cómo migrar la base de datos de SQL Server Compact a SQL Server Express y SQL Server.</span><span class="sxs-lookup"><span data-stu-id="281e3-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="281e3-156">[Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="281e3-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>