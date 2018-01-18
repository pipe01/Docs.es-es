# <a name="adding-validation"></a><span data-ttu-id="57559-101">Adición de validación</span><span class="sxs-lookup"><span data-stu-id="57559-101">Adding validation</span></span>

<span data-ttu-id="57559-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="57559-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="57559-103">En esta sección se agregará lógica de validación al modelo `Movie` y se asegurará de que las reglas de validación se aplican siempre que un usuario crea o edita una película.</span><span class="sxs-lookup"><span data-stu-id="57559-103">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="57559-104">Respetar el principio DRY</span><span class="sxs-lookup"><span data-stu-id="57559-104">Keeping things DRY</span></span>

<span data-ttu-id="57559-105">Uno de los principios de diseño de MVC es [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("Una vez y solo una").</span><span class="sxs-lookup"><span data-stu-id="57559-105">One of the design tenets of MVC is [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("Don't Repeat Yourself").</span></span> <span data-ttu-id="57559-106">ASP.NET MVC le anima a que especifique la función o el comportamiento una sola vez y luego lo refleje en el resto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57559-106">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an app.</span></span> <span data-ttu-id="57559-107">Esto reduce la cantidad de código que necesita escribir y hace que el código que escribe sea menos propenso a errores, así como más fácil probar y de mantener.</span><span class="sxs-lookup"><span data-stu-id="57559-107">This reduces the amount of code you need to write and makes the code you do write less error prone, easier to test, and easier to maintain.</span></span>

<span data-ttu-id="57559-108">La compatibilidad de validación proporcionada por MVC y Entity Framework Core Code First es un buen ejemplo del principio DRY.</span><span class="sxs-lookup"><span data-stu-id="57559-108">The validation support provided by MVC and Entity Framework Core Code First is a good example of the DRY principle in action.</span></span> <span data-ttu-id="57559-109">Puede especificar las reglas de validación mediante declaración en un lugar (en la clase del modelo) y las reglas se aplican en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57559-109">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the app.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="57559-110">Adición de reglas de validación al modelo de película</span><span class="sxs-lookup"><span data-stu-id="57559-110">Adding validation rules to the movie model</span></span>

<span data-ttu-id="57559-111">Abra el archivo *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="57559-111">Open the *Movie.cs* file.</span></span> <span data-ttu-id="57559-112">DataAnnotations proporciona un conjunto integrado de atributos de validación que se aplican mediante declaración a cualquier clase o propiedad.</span><span class="sxs-lookup"><span data-stu-id="57559-112">DataAnnotations provides a built-in set of validation attributes that you apply declaratively to any class or property.</span></span> <span data-ttu-id="57559-113">(También contiene atributos de formato como `DataType`, que ayudan a aplicar formato y no proporcionan validación).</span><span class="sxs-lookup"><span data-stu-id="57559-113">(It also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.)</span></span>

<span data-ttu-id="57559-114">Actualice la clase `Movie` para aprovechar los atributos de validación integrados `Required`, `StringLength`, `RegularExpression` y `Range`.</span><span class="sxs-lookup"><span data-stu-id="57559-114">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="57559-115">Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="57559-115">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="57559-116">Los atributos `Required` y `MinimumLength` indican que una propiedad debe tener un valor, pero nada evita que un usuario escriba espacios en blanco para satisfacer esta validación.</span><span class="sxs-lookup"><span data-stu-id="57559-116">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span> <span data-ttu-id="57559-117">El atributo `RegularExpression` se usa para limitar los caracteres que se pueden escribir.</span><span class="sxs-lookup"><span data-stu-id="57559-117">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="57559-118">En el código anterior, `Genre` y `Rating` solamente pueden usar letras (no se permiten espacios en blanco, números ni caracteres especiales).</span><span class="sxs-lookup"><span data-stu-id="57559-118">In the code above, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="57559-119">El atributo `Range` restringe un valor a un intervalo determinado.</span><span class="sxs-lookup"><span data-stu-id="57559-119">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="57559-120">El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="57559-120">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="57559-121">Los tipos de valor (como `decimal`, `int`, `float`, `DateTime`) son intrínsecamente necesarios y no necesitan el atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="57559-121">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="57559-122">Cuando ASP.NET aplica automáticamente reglas de validación, logramos que la aplicación sea más sólida.</span><span class="sxs-lookup"><span data-stu-id="57559-122">Having validation rules automatically enforced by ASP.NET helps make your app more robust.</span></span> <span data-ttu-id="57559-123">También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.</span><span class="sxs-lookup"><span data-stu-id="57559-123">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

## <a name="validation-error-ui-in-mvc"></a><span data-ttu-id="57559-124">Interfaz de usuario de error de validación en MVC</span><span class="sxs-lookup"><span data-stu-id="57559-124">Validation Error UI in MVC</span></span>

<span data-ttu-id="57559-125">Ejecute la aplicación y navegue al controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="57559-125">Run the app and navigate to the Movies controller.</span></span>

<span data-ttu-id="57559-126">Pulse el vínculo **Crear nueva** para agregar una nueva película.</span><span class="sxs-lookup"><span data-stu-id="57559-126">Tap the **Create New** link to add a new movie.</span></span> <span data-ttu-id="57559-127">Rellene el formulario con algunos valores no válidos.</span><span class="sxs-lookup"><span data-stu-id="57559-127">Fill out the form with some invalid values.</span></span> <span data-ttu-id="57559-128">En cuanto la validación del lado cliente de jQuery detecta el problema, muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="57559-128">As soon as jQuery client side validation detects the error, it displays an error message.</span></span>

![Formulario de vista de películas con varios errores de validación del lado cliente de jQuery](../../tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="57559-130">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="57559-130">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="57559-131">Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57559-131">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="57559-132">Consulte el [problema 4076 de GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obtener instrucciones sobre cómo agregar la coma decimal.</span><span class="sxs-lookup"><span data-stu-id="57559-132">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span> 

<span data-ttu-id="57559-133">Observe cómo el formulario presenta automáticamente un mensaje de error de validación adecuado en cada campo que contiene un valor no válido.</span><span class="sxs-lookup"><span data-stu-id="57559-133">Notice how the form has automatically rendered an appropriate validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="57559-134">Los errores se aplican en el lado cliente (con JavaScript y jQuery) y en el lado servidor (cuando un usuario tiene JavaScript deshabilitado).</span><span class="sxs-lookup"><span data-stu-id="57559-134">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="57559-135">Una ventaja importante es que no fue necesario cambiar ni una sola línea de código en la clase `MoviesController` o en la vista *Create.cshtml* para habilitar esta interfaz de usuario de validación.</span><span class="sxs-lookup"><span data-stu-id="57559-135">A significant benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="57559-136">El controlador y las vistas que creó en pasos anteriores de este tutorial seleccionaron automáticamente las reglas de validación que especificó mediante atributos de validación en las propiedades de la clase del modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="57559-136">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span> <span data-ttu-id="57559-137">Pruebe la aplicación mediante el método de acción `Edit` y se aplicará la misma validación.</span><span class="sxs-lookup"><span data-stu-id="57559-137">Test validation using the `Edit` action method, and the same validation is applied.</span></span>

<span data-ttu-id="57559-138">Los datos del formulario no se envían al servidor hasta que no hay ningún error de validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="57559-138">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="57559-139">Puede comprobarlo colocando un punto de interrupción en el método `HTTP Post` mediante la [herramienta Fiddler](http://www.telerik.com/fiddler) o las [herramientas de desarrollo F12](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="57559-139">You can verify this by putting a break point in the `HTTP Post` method, by using the [Fiddler tool](http://www.telerik.com/fiddler) , or the [F12 Developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>

## <a name="how-validation-works"></a><span data-ttu-id="57559-140">Cómo funciona la validación</span><span class="sxs-lookup"><span data-stu-id="57559-140">How validation works</span></span>

<span data-ttu-id="57559-141">Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas.</span><span class="sxs-lookup"><span data-stu-id="57559-141">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="57559-142">En el código siguiente se muestran los dos métodos `Create`.</span><span class="sxs-lookup"><span data-stu-id="57559-142">The following code shows the two `Create` methods.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

<span data-ttu-id="57559-143">El primer método de acción `Create` (HTTP GET) muestra el formulario de creación inicial.</span><span class="sxs-lookup"><span data-stu-id="57559-143">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="57559-144">La segunda versión (`[HttpPost]`) controla el envío de formulario.</span><span class="sxs-lookup"><span data-stu-id="57559-144">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="57559-145">El segundo método `Create` (la versión `[HttpPost]`) llama a `ModelState.IsValid` para comprobar si la película tiene errores de validación.</span><span class="sxs-lookup"><span data-stu-id="57559-145">The second `Create` method (The `[HttpPost]` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="57559-146">Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto.</span><span class="sxs-lookup"><span data-stu-id="57559-146">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="57559-147">Si el objeto tiene errores de validación, el método `Create` vuelve a mostrar el formulario.</span><span class="sxs-lookup"><span data-stu-id="57559-147">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="57559-148">Si no hay ningún error, el método guarda la nueva película en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="57559-148">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="57559-149">En nuestro ejemplo de película, el formulario no se publica en el servidor cuando se detectan errores de validación del lado cliente; cuando hay errores de validación en el lado cliente, no se llama nunca al segundo método `Create`.</span><span class="sxs-lookup"><span data-stu-id="57559-149">In our movie example, the form is not posted to the server when there are validation errors detected on the client side; the second `Create` method is never called when there are client side validation errors.</span></span> <span data-ttu-id="57559-150">Si deshabilita JavaScript en el explorador, se deshabilita también la validación del cliente y puede probar si el método `Create` HTTP POST `ModelState.IsValid` detecta errores de validación.</span><span class="sxs-lookup"><span data-stu-id="57559-150">If you disable JavaScript in your browser, client validation is disabled and you can test the HTTP POST `Create` method `ModelState.IsValid` detecting any validation errors.</span></span>

<span data-ttu-id="57559-151">Puede establecer un punto de interrupción en el método `[HttpPost] Create` y comprobar si nunca se llama al método. La validación del lado cliente no enviará los datos del formulario cuando se detecten errores de validación.</span><span class="sxs-lookup"><span data-stu-id="57559-151">You can set a break point in the `[HttpPost] Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="57559-152">Si deshabilita JavaScript en el explorador y después envía el formulario con errores, se alcanzará el punto de interrupción.</span><span class="sxs-lookup"><span data-stu-id="57559-152">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="57559-153">Puede seguir obteniendo validación completa sin JavaScript.</span><span class="sxs-lookup"><span data-stu-id="57559-153">You still get full validation without JavaScript.</span></span> 

<span data-ttu-id="57559-154">En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Firefox.</span><span class="sxs-lookup"><span data-stu-id="57559-154">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![Firefox: en la pestaña Contenido de Opciones, desactive la casilla Habilitar JavaScript.](../../tutorials/first-mvc-app/validation/_static/ff.png)

<span data-ttu-id="57559-156">En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Chrome.</span><span class="sxs-lookup"><span data-stu-id="57559-156">The following image shows how to disable JavaScript in the Chrome browser.</span></span>

![Google Chrome: en la sección JavaScript de Configuración de contenido, seleccione la opción para no permitir a ningún sitio ejecutar JavaScript.](../../tutorials/first-mvc-app/validation/_static/chrome.png)

<span data-ttu-id="57559-158">Después de deshabilitar JavaScript, publique los datos no válidos y siga los pasos del depurador.</span><span class="sxs-lookup"><span data-stu-id="57559-158">After you disable JavaScript, post invalid data and step through the debugger.</span></span>

![Durante la depuración en una publicación de datos no válidos, Intellisense en ModelState.IsValid muestra que el valor es falso.](../../tutorials/first-mvc-app/validation/_static/ms.png)

<span data-ttu-id="57559-160">Abajo se muestra una parte de la plantilla de vista *Create.cshtml* a la que se aplicó scaffolding en un paso anterior de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="57559-160">Below is portion of the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="57559-161">Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.</span><span class="sxs-lookup"><span data-stu-id="57559-161">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

<span data-ttu-id="57559-162">La [aplicación auxiliar de etiquetas de entrada](xref:mvc/views/working-with-forms) usa los atributos [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="57559-162">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client side.</span></span> <span data-ttu-id="57559-163">La [aplicación auxiliar de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) muestra errores de validación.</span><span class="sxs-lookup"><span data-stu-id="57559-163">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="57559-164">Para más información, vea [Introduction to model validation in ASP.NET Core MVC](xref:mvc/models/validation) (Introducción a la validación de modelos en ASP.NET Core MVC).</span><span class="sxs-lookup"><span data-stu-id="57559-164">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="57559-165">Lo realmente bueno de este enfoque es que ni el controlador ni la plantilla de vista `Create` saben que las reglas de validación actuales se están aplicando ni conocen los mensajes de error específicos que se muestran.</span><span class="sxs-lookup"><span data-stu-id="57559-165">What's really nice about this approach is that neither the controller nor the `Create` view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="57559-166">Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="57559-166">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="57559-167">Estas mismas reglas de validación se aplican automáticamente a la vista `Edit` y a cualquier otra vista de plantillas creada que edite el modelo.</span><span class="sxs-lookup"><span data-stu-id="57559-167">These same validation rules are automatically applied to the `Edit` view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="57559-168">Cuando necesite cambiar la lógica de validación, puede hacerlo exactamente en un solo lugar mediante la adición de atributos de validación al modelo (en este ejemplo, la clase `Movie`).</span><span class="sxs-lookup"><span data-stu-id="57559-168">When you need to change validation logic, you can do so in exactly one place by adding validation attributes to the model (in this example, the `Movie` class).</span></span> <span data-ttu-id="57559-169">No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes.</span><span class="sxs-lookup"><span data-stu-id="57559-169">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="57559-170">Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar.</span><span class="sxs-lookup"><span data-stu-id="57559-170">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="57559-171">También significa que respeta totalmente el principio DRY.</span><span class="sxs-lookup"><span data-stu-id="57559-171">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="57559-172">Uso de atributos DataType</span><span class="sxs-lookup"><span data-stu-id="57559-172">Using DataType Attributes</span></span>

<span data-ttu-id="57559-173">Abra el archivo *Movie.cs* y examine la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="57559-173">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="57559-174">El espacio de nombres `System.ComponentModel.DataAnnotations` proporciona atributos de formato además del conjunto integrado de atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="57559-174">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="57559-175">Ya hemos aplicado un valor de enumeración `DataType` en la fecha de lanzamiento y los campos de precio.</span><span class="sxs-lookup"><span data-stu-id="57559-175">We've already applied a `DataType` enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="57559-176">En el código siguiente se muestran las propiedades `ReleaseDate` y `Price` con el atributo `DataType` adecuado.</span><span class="sxs-lookup"><span data-stu-id="57559-176">The following code shows the `ReleaseDate` and `Price` properties with the appropriate `DataType` attribute.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="57559-177">Los atributos `DataType` solo proporcionan sugerencias para que el motor de vista aplique formato a los datos (y suministre atributos como `<a>` para las direcciones URL y `<a href="mailto:EmailAddress.com">` para el correo electrónico).</span><span class="sxs-lookup"><span data-stu-id="57559-177">The `DataType` attributes only provide hints for the view engine to format the data (and supply attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email.</span></span> <span data-ttu-id="57559-178">Use el atributo `RegularExpression` para validar el formato de los datos.</span><span class="sxs-lookup"><span data-stu-id="57559-178">You can use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="57559-179">El atributo `DataType` no es un atributo de validación, sino que se usa para especificar un tipo de datos más específico que el tipo intrínseco de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="57559-179">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type, they are not validation attributes.</span></span> <span data-ttu-id="57559-180">En este caso solo queremos realizar un seguimiento de la fecha, no la hora.</span><span class="sxs-lookup"><span data-stu-id="57559-180">In this case we only want to keep track of the date, not the time.</span></span> <span data-ttu-id="57559-181">La enumeración `DataType` proporciona muchos tipos de datos, como Date (Fecha), Time (Hora), PhoneNumber (Número de teléfono), Currency (Moneda), EmailAddress (Dirección de correo electrónico), etc.</span><span class="sxs-lookup"><span data-stu-id="57559-181">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress and more.</span></span> <span data-ttu-id="57559-182">El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="57559-182">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="57559-183">Por ejemplo, se puede crear un vínculo `mailto:` para `DataType.EmailAddress` y se puede proporcionar un selector de datos para `DataType.Date` en exploradores compatibles con HTML5.</span><span class="sxs-lookup"><span data-stu-id="57559-183">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="57559-184">Los atributos `DataType` emiten atributos HTML 5 `data-` (se pronuncia "datos dash") que los exploradores HTML 5 pueden comprender.</span><span class="sxs-lookup"><span data-stu-id="57559-184">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="57559-185">Los atributos `DataType` **no** proporcionan ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="57559-185">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="57559-186">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="57559-186">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="57559-187">De manera predeterminada, el campo de datos se muestra según los formatos predeterminados basados en el elemento `CultureInfo` del servidor.</span><span class="sxs-lookup"><span data-stu-id="57559-187">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="57559-188">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="57559-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="57559-189">El valor `ApplyFormatInEditMode` especifica que el formato se debe aplicar también cuando el valor se muestra en un cuadro de texto para su edición.</span><span class="sxs-lookup"><span data-stu-id="57559-189">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="57559-190">En algunos campos este comportamiento puede no ser conveniente. Por poner un ejemplo, es probable que con valores de moneda no se quiera que el símbolo de la divisa se incluya en el cuadro de texto editable.</span><span class="sxs-lookup"><span data-stu-id="57559-190">(You might not want that for some fields — for example, for currency values, you probably do not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="57559-191">El atributo `DisplayFormat` puede usarse por sí solo, pero normalmente se recomienda usar el atributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="57559-191">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="57559-192">El atributo `DataType` transmite la semántica de los datos en contraposición a cómo se representa en una pantalla y ofrece las siguientes ventajas que no proporciona DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="57559-192">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="57559-193">El explorador puede habilitar características de HTML5 (por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado según la configuración regional, vínculos de correo electrónico, etc.).</span><span class="sxs-lookup"><span data-stu-id="57559-193">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>

* <span data-ttu-id="57559-194">De manera predeterminada, el explorador representa los datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="57559-194">By default, the browser will render data using the correct format based on your locale.</span></span>

* <span data-ttu-id="57559-195">El atributo `DataType` puede habilitar MVC para que elija la plantilla de campo adecuada para representar los datos (`DisplayFormat`, si se usa por sí solo, usa la plantilla de cadena).</span><span class="sxs-lookup"><span data-stu-id="57559-195">The `DataType` attribute can enable MVC to choose the right field template to render the data (the `DisplayFormat` if used by itself uses the string template).</span></span>

> [!NOTE]
> <span data-ttu-id="57559-196">La validación de jQuery no funciona con el atributo `Range` ni `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="57559-196">jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="57559-197">Por ejemplo, el código siguiente siempre muestra un error de validación del lado cliente, incluso cuando la fecha está en el intervalo especificado:</span><span class="sxs-lookup"><span data-stu-id="57559-197">For example, the following code will always display a client side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="57559-198">Debe deshabilitar la validación de fechas de jQuery para usar el atributo `Range` con `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="57559-198">You will need to disable jQuery date validation to use the `Range` attribute with `DateTime`.</span></span> <span data-ttu-id="57559-199">Por lo general no se recomienda compilar fechas fijas en los modelos, así que desaconseja usar el atributo `Range` y `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="57559-199">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="57559-200">El código siguiente muestra la combinación de atributos en una línea:</span><span class="sxs-lookup"><span data-stu-id="57559-200">The following code shows combining attributes on one line:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="57559-201">En la siguiente parte de la serie de tutoriales, revisaremos la aplicación y realizaremos algunas mejoras a los métodos `Details` y `Delete` generados automáticamente.</span><span class="sxs-lookup"><span data-stu-id="57559-201">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="57559-202">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="57559-202">Additional resources</span></span>

* [<span data-ttu-id="57559-203">Trabajar con formularios</span><span class="sxs-lookup"><span data-stu-id="57559-203">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="57559-204">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="57559-204">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="57559-205">Introducción a las aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="57559-205">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="57559-206">Creación de aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="57559-206">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)