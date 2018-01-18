---
title: "Mediante la autenticación de Cookie sin identidad principal de ASP.NET"
author: rick-anderson
description: "Obtener una explicación del uso de autenticación con cookies sin ASP.NET Core Identity"
keywords: "Núcleo de ASP.NET, las cookies"
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 6279d3b4ac3be102449089dc66eeeb0495cfc4c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="6a1ac-104">Mediante la autenticación de Cookie sin identidad principal de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6a1ac-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="6a1ac-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6a1ac-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6a1ac-106">Como ha visto en los temas de autenticación anteriores, [ASP.NET Core Identity](xref:security/authentication/identity) es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-106">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="6a1ac-107">Sin embargo, puede que desee utilizar su propia lógica de autenticación personalizada con la autenticación basada en cookies a veces.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-107">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="6a1ac-108">Puede usar la autenticación basada en cookies como un proveedor de autenticación independiente sin ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-108">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="6a1ac-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6a1ac-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6a1ac-110">Para obtener información sobre migración autenticación basada en cookies de ASP.NET Core 1.x a 2.0, consulte [migrar autenticación e identidad al tema principal de ASP.NET 2.0 (autenticación basada en cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-110">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="6a1ac-111">Configuración</span><span class="sxs-lookup"><span data-stu-id="6a1ac-111">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a1ac-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a1ac-113">Si no está usando la [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instale la versión 2.0 de la [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-113">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="6a1ac-114">En el `ConfigureServices` (método), crear el servicio de Middleware de autenticación con el `AddAuthentication` y `AddCookie` métodos:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-114">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="6a1ac-115">`AuthenticationScheme`pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-115">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="6a1ac-116">`AuthenticationScheme`es útil cuando hay varias instancias de autenticación con cookies y desea [autorizarse con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-116">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="6a1ac-117">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-117">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="6a1ac-118">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-118">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="6a1ac-119">En el `Configure` método, use la `UseAuthentication` método que se invoca el Middleware de autenticación que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-119">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="6a1ac-120">Llame a la `UseAuthentication` método antes de llamar a `AddMvcWithDefaultRoute` en una aplicación MVC o `AddMvc` en una aplicación de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-120">Call the `UseAuthentication` method before calling `AddMvcWithDefaultRoute` in an MVC app or `AddMvc` in a Razor Pages app:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="6a1ac-121">**Opciones de AddCookie**</span><span class="sxs-lookup"><span data-stu-id="6a1ac-121">**AddCookie Options**</span></span>

<span data-ttu-id="6a1ac-122">El [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) clase se utiliza para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-122">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="6a1ac-123">Opción</span><span class="sxs-lookup"><span data-stu-id="6a1ac-123">Option</span></span> | <span data-ttu-id="6a1ac-124">Descripción</span><span class="sxs-lookup"><span data-stu-id="6a1ac-124">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="6a1ac-125">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="6a1ac-125">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-126">Proporciona la ruta de acceso para suministrar con un 302 encontrado (redireccionamiento de la dirección URL) cuando se desencadena por `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-126">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="6a1ac-127">El valor predeterminado es `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-127">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="6a1ac-128">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="6a1ac-128">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-129">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creados por el servicio de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-129">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="6a1ac-130">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="6a1ac-130">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-131">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-131">The domain name where the cookie is served.</span></span> <span data-ttu-id="6a1ac-132">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-132">By default, this is the host name of the request.</span></span> <span data-ttu-id="6a1ac-133">El explorador sólo envía la cookie en las solicitudes a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-133">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="6a1ac-134">Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-134">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="6a1ac-135">Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-135">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="6a1ac-136">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="6a1ac-136">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-137">Obtiene o establece el tiempo de vida de una cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-137">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="6a1ac-138">Actualmente, esta opción no ops y quedarán obsoleta en ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-138">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="6a1ac-139">Use la `ExpireTimeSpan` opción para establecer la expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-139">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="6a1ac-140">Para obtener más información, consulte [aclarar el comportamiento de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-140">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="6a1ac-141">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="6a1ac-141">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-142">Una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-142">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="6a1ac-143">Si cambia este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) una vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-143">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="6a1ac-144">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-144">The default value is `true`.</span></span> |
| [<span data-ttu-id="6a1ac-145">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="6a1ac-145">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-146">Establece el nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-146">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="6a1ac-147">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="6a1ac-147">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-148">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-148">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="6a1ac-149">Si tiene aplicaciones que se ejecutan en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-149">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="6a1ac-150">Al hacerlo, la cookie solo está disponible en las solicitudes a `/app1` y cualquier aplicación aparecen debajo de él.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-150">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="6a1ac-151">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="6a1ac-151">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-152">Indica si el explorador debe permitir la cookie que se adjuntará a sólo las solicitudes del mismo sitio (`SameSiteMode.Strict`) o solicitudes entre sitios mediante métodos de prueba de errores HTTP y solicitudes del mismo sitio (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-152">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="6a1ac-153">Cuando se establece en `SameSiteMode.None`, no se establece el valor del encabezado de cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-153">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="6a1ac-154">Tenga en cuenta que [Middleware de cookies directiva](#cookie-policy-middleware) podría sobrescribir el valor que se proporcione.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-154">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="6a1ac-155">Para admitir la autenticación de OAuth, el valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-155">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="6a1ac-156">Para obtener más información, consulte [roto debido a la directiva de SameSite cookie de autenticación de OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-156">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="6a1ac-157">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="6a1ac-157">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-158">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-158">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="6a1ac-159">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-159">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="6a1ac-160">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="6a1ac-160">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-161">Establece el `DataProtectionProvider` que se utiliza para crear el valor predeterminado `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-161">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="6a1ac-162">Si el `TicketDataFormat` propiedad está establecida, el `DataProtectionProvider` opción no se utiliza.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-162">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="6a1ac-163">Si no se proporciona, se utiliza el proveedor de protección de datos de la aplicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-163">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="6a1ac-164">Eventos</span><span class="sxs-lookup"><span data-stu-id="6a1ac-164">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-165">El controlador llama a métodos en el proveedor que proporcionan el control de la aplicación en determinados puntos de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-165">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="6a1ac-166">Si `Events` no siempre, se proporciona una instancia predeterminada que no hace nada cuando se llaman a los métodos.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-166">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="6a1ac-167">EventsType</span><span class="sxs-lookup"><span data-stu-id="6a1ac-167">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-168">Utiliza como el tipo de servicio para obtener el `Events` instancia en lugar de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-168">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="6a1ac-169">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="6a1ac-169">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-170">El `TimeSpan` tras el cual expira el vale de autenticación que se almacena dentro de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-170">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="6a1ac-171">`ExpireTimeSpan`se agrega a la hora actual para crear la fecha de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-171">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="6a1ac-172">El `ExpiredTimeSpan` siempre que el valor sea en el cifrado AuthTicket verificada por el servidor.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-172">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="6a1ac-173">También puede ir a la [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) encabezado, pero solo si `IsPersistent` se establece.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-173">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="6a1ac-174">Para establecer `IsPersistent` a `true`, configurar la [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) pasado a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-174">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="6a1ac-175">El valor predeterminado de `ExpireTimeSpan` es 14 días.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-175">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="6a1ac-176">LoginPath</span><span class="sxs-lookup"><span data-stu-id="6a1ac-176">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-177">Proporciona la ruta de acceso para suministrar con un 302 encontrado (redireccionamiento de la dirección URL) cuando se desencadena por `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-177">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="6a1ac-178">La dirección URL actual que generó el 401 se agrega a la `LoginPath` como un parámetro de cadena de consulta denominado por la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-178">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="6a1ac-179">Una vez una solicitud para la `LoginPath` concede una nueva identidad de inicio de sesión, la `ReturnUrlParameter` valor se utiliza para redirigir el explorador a la dirección URL que produjo el código de estado sin autorización original.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-179">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="6a1ac-180">El valor predeterminado es `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-180">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="6a1ac-181">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="6a1ac-181">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-182">Si el `LogoutPath` se proporciona al controlador, a continuación, redirige una solicitud a dicha ruta de acceso en función del valor de la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-182">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="6a1ac-183">El valor predeterminado es `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-183">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="6a1ac-184">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="6a1ac-184">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-185">Determina el nombre del parámetro de cadena de consulta que se anexa el controlador para una respuesta 302 de Found (redireccionamiento de la dirección URL).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-185">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="6a1ac-186">`ReturnUrlParameter`se utiliza cuando llega una solicitud en el `LoginPath` o `LogoutPath` para devolver el explorador a la dirección URL original después de realiza la acción de inicio de sesión o cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-186">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="6a1ac-187">El valor predeterminado es `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-187">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="6a1ac-188">SessionStore</span><span class="sxs-lookup"><span data-stu-id="6a1ac-188">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-189">Contenedor opcional que se utiliza para almacenar la identidad todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-189">An optional container used to store identity across requests.</span></span> <span data-ttu-id="6a1ac-190">Cuando se utiliza, solo un identificador de sesión se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-190">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="6a1ac-191">`SessionStore`puede utilizarse para mitigar los posibles problemas con identidades grandes.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-191">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="6a1ac-192">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="6a1ac-192">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-193">Una marca que indica si se debe emitir una nueva cookie con una fecha de caducidad actualizada dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-193">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="6a1ac-194">Esto puede suceder en cualquier solicitud que el período de expiración de cookie actual es más del 50% expira.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-194">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="6a1ac-195">La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-195">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="6a1ac-196">Un [tiempo de expiración de cookie absoluta](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-196">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="6a1ac-197">Una hora de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-197">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="6a1ac-198">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-198">The default value is `true`.</span></span> |
| [<span data-ttu-id="6a1ac-199">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="6a1ac-199">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-200">El `TicketDataFormat` se usa para proteger y desproteger la identidad y otras propiedades que se almacenan en el valor de cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-200">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="6a1ac-201">Si no se proporciona un `TicketDataFormat` se crea utilizando el [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-201">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="6a1ac-202">Validar</span><span class="sxs-lookup"><span data-stu-id="6a1ac-202">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="6a1ac-203">Método que compruebe que las opciones son válidas.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-203">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="6a1ac-204">Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-204">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a1ac-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a1ac-206">ASP.NET Core 1.x usa cookies [middleware](xref:fundamentals/middleware) que serializa una entidad de seguridad de usuario en una cookie cifrada.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-206">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="6a1ac-207">En solicitudes posteriores, la cookie se valida y se vuelve a crear la entidad de seguridad y se asigna a la `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-207">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="6a1ac-208">Instalar el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete de NuGet en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-208">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="6a1ac-209">Este paquete contiene el middleware de cookies.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-209">This package contains the cookie middleware.</span></span>

<span data-ttu-id="6a1ac-210">Use la `UseCookieAuthentication` método en el `Configure` método en su *Startup.cs* archivo antes de `UseMvc` o `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-210">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="6a1ac-211">**Opciones de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="6a1ac-211">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="6a1ac-212">El [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) clase se utiliza para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-212">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="6a1ac-213">Opción</span><span class="sxs-lookup"><span data-stu-id="6a1ac-213">Option</span></span> | <span data-ttu-id="6a1ac-214">Descripción</span><span class="sxs-lookup"><span data-stu-id="6a1ac-214">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="6a1ac-215">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="6a1ac-215">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-216">Establece el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-216">Sets the authentication scheme.</span></span> <span data-ttu-id="6a1ac-217">`AuthenticationScheme`es útil cuando hay varias instancias de autenticación y desea [autorizarse con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-217">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="6a1ac-218">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-218">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="6a1ac-219">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-219">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="6a1ac-220">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="6a1ac-220">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-221">Establece un valor para indicar que la autenticación con cookies debe ejecutarse en cada solicitud y que intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-221">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="6a1ac-222">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="6a1ac-222">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-223">Si es true, el middleware de autenticación controla desafíos automática.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-223">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="6a1ac-224">Si false, el middleware de autenticación sólo altera las respuestas cuando indique explícitamente la `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-224">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="6a1ac-225">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="6a1ac-225">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-226">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en cualquier notificación creado por el middleware de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-226">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="6a1ac-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="6a1ac-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-228">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-228">The domain name where the cookie is served.</span></span> <span data-ttu-id="6a1ac-229">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-229">By default, this is the host name of the request.</span></span> <span data-ttu-id="6a1ac-230">El explorador sólo sirve la cookie a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-230">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="6a1ac-231">Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-231">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="6a1ac-232">Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-232">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="6a1ac-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="6a1ac-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-234">Una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-234">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="6a1ac-235">Si cambia este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) una vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-235">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="6a1ac-236">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-236">The default value is `true`.</span></span> |
| [<span data-ttu-id="6a1ac-237">CookiePath</span><span class="sxs-lookup"><span data-stu-id="6a1ac-237">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-238">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-238">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="6a1ac-239">Si tiene aplicaciones que se ejecutan en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-239">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="6a1ac-240">Al hacerlo, la cookie solo está disponible en las solicitudes a `/app1` y cualquier aplicación aparecen debajo de él.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-240">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="6a1ac-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="6a1ac-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-242">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-242">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="6a1ac-243">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-243">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="6a1ac-244">Descripción</span><span class="sxs-lookup"><span data-stu-id="6a1ac-244">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-245">Información adicional sobre el tipo de autenticación que debe ponerse a disposición de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-245">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="6a1ac-246">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="6a1ac-246">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-247">El `TimeSpan` tras el cual expira el vale de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-247">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="6a1ac-248">Se agrega a la hora actual para crear la fecha de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-248">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="6a1ac-249">Usar `ExpireTimeSpan`, debe establecer `IsPersistent` a `true` en el `AuthenticationProperties` pasado a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-249">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="6a1ac-250">El valor predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-250">The default value is 14 days.</span></span> |
| [<span data-ttu-id="6a1ac-251">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="6a1ac-251">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="6a1ac-252">Una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-252">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="6a1ac-253">La nueva hora de exipiration se mueve hacia delante como la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-253">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="6a1ac-254">Un [tiempo de expiración de cookie absoluta](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-254">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="6a1ac-255">Una hora de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-255">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="6a1ac-256">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-256">The default value is `true`.</span></span> |

<span data-ttu-id="6a1ac-257">Establecer `CookieAuthenticationOptions` para el Middleware de autenticación de Cookie en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-257">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="6a1ac-258">Middleware de cookies directiva.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-258">Cookie Policy Middleware</span></span>

<span data-ttu-id="6a1ac-259">[Middleware de cookies directiva](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita las capacidades de directiva de cookie de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-259">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="6a1ac-260">Agregar el middleware a la canalización de procesamiento de la aplicación es el orden de minúsculas; solo afecta a los componentes registrados después de él en la canalización.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-260">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="6a1ac-261">El [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) proporcionado para el Middleware de directiva de Cookie le permiten controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de cookie cuando se agrega o se eliminan las cookies.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-261">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="6a1ac-262">Propiedad</span><span class="sxs-lookup"><span data-stu-id="6a1ac-262">Property</span></span> | <span data-ttu-id="6a1ac-263">Descripción</span><span class="sxs-lookup"><span data-stu-id="6a1ac-263">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="6a1ac-264">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="6a1ac-264">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="6a1ac-265">Afecta a si las cookies deben estar HttpOnly, que es una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-265">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="6a1ac-266">El valor predeterminado es `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-266">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="6a1ac-267">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="6a1ac-267">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="6a1ac-268">Afecta al atributo del mismo sitio de la cookie (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-268">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="6a1ac-269">El valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-269">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="6a1ac-270">Esta opción está disponible para el núcleo de ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-270">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="6a1ac-271">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="6a1ac-271">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="6a1ac-272">Se llama cuando se anexa una cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-272">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="6a1ac-273">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="6a1ac-273">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="6a1ac-274">Se llama cuando se elimina una cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-274">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="6a1ac-275">Proteger</span><span class="sxs-lookup"><span data-stu-id="6a1ac-275">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="6a1ac-276">Determina si las cookies deben estar seguro.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-276">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="6a1ac-277">El valor predeterminado es `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-277">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="6a1ac-278">**MinimumSameSitePolicy** (ASP.NET 2.0 + sólo principal)</span><span class="sxs-lookup"><span data-stu-id="6a1ac-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="6a1ac-279">El valor predeterminado `MinimumSameSitePolicy` valor es `SameSiteMode.Lax` para permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-279">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="6a1ac-280">Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-280">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="6a1ac-281">Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no confían en el procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-281">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="6a1ac-282">La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-282">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="6a1ac-283">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="6a1ac-283">MinimumSameSitePolicy</span></span> | <span data-ttu-id="6a1ac-284">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="6a1ac-284">Cookie.SameSite</span></span> | <span data-ttu-id="6a1ac-285">Configuración de Cookie.SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="6a1ac-285">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="6a1ac-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6a1ac-286">SameSiteMode.None</span></span>     | <span data-ttu-id="6a1ac-287">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6a1ac-287">SameSiteMode.None</span></span><br><span data-ttu-id="6a1ac-288">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-288">SameSiteMode.Lax</span></span><br><span data-ttu-id="6a1ac-289">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-289">SameSiteMode.Strict</span></span> | <span data-ttu-id="6a1ac-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6a1ac-290">SameSiteMode.None</span></span><br><span data-ttu-id="6a1ac-291">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-291">SameSiteMode.Lax</span></span><br><span data-ttu-id="6a1ac-292">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-292">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="6a1ac-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-293">SameSiteMode.Lax</span></span>      | <span data-ttu-id="6a1ac-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6a1ac-294">SameSiteMode.None</span></span><br><span data-ttu-id="6a1ac-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="6a1ac-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-296">SameSiteMode.Strict</span></span> | <span data-ttu-id="6a1ac-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="6a1ac-298">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-298">SameSiteMode.Lax</span></span><br><span data-ttu-id="6a1ac-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-299">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="6a1ac-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-300">SameSiteMode.Strict</span></span>   | <span data-ttu-id="6a1ac-301">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6a1ac-301">SameSiteMode.None</span></span><br><span data-ttu-id="6a1ac-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6a1ac-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="6a1ac-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-303">SameSiteMode.Strict</span></span> | <span data-ttu-id="6a1ac-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="6a1ac-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-305">SameSiteMode.Strict</span></span><br><span data-ttu-id="6a1ac-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6a1ac-306">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="6a1ac-307">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="6a1ac-307">Creating an authentication cookie</span></span>

<span data-ttu-id="6a1ac-308">Para crear una cookie que contiene información de usuario, debe construir un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-308">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="6a1ac-309">La información de usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-309">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a1ac-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a1ac-311">Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-311">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a1ac-312">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a1ac-313">Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="6a1ac-314">`SignInAsync`crea una cookie cifrada y lo agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="6a1ac-315">Si no se especifica un `AuthenticationScheme`, se utiliza el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="6a1ac-316">Tras los bastidores, el cifrado usado es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="6a1ac-317">Si aloja la aplicación en varios equipos, equilibrio de carga entre las aplicaciones o usar una granja de servidores web, debe [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo anillo de clave y el identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="6a1ac-318">Cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="6a1ac-318">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a1ac-319">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-319">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a1ac-320">Para cerrar la sesión del usuario actual y eliminar las cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="6a1ac-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a1ac-321">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-321">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a1ac-322">Para cerrar la sesión del usuario actual y eliminar las cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="6a1ac-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="6a1ac-323">Si no está usando `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que usó al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="6a1ac-324">En caso contrario, se utiliza el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-324">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="6a1ac-325">Reaccionar ante los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="6a1ac-325">Reacting to back-end changes</span></span>

<span data-ttu-id="6a1ac-326">Una vez que se crea una cookie, se convierte en el único origen de identidad.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="6a1ac-327">Incluso si se deshabilita a un usuario en los sistemas back-end, el sistema de autenticación de cookie no tiene ningún conocimiento de este, y un usuario permanece ha iniciado sesión como su cookie es válida.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="6a1ac-328">El [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) eventos en ASP.NET Core 2.x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método en ASP.NET Core 1.x puede usarse para interceptar y reemplazar la validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="6a1ac-329">Este enfoque reduce el riesgo de revocados a los usuarios obtener acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="6a1ac-330">Un enfoque de validación de la cookie se basa en realizar el seguimiento de cuándo se ha modificado la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="6a1ac-331">Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si su cookie sigue siendo válido.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-331">If the database hasn't been changed since the user's cookie was issued, there is no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="6a1ac-332">Para implementar este escenario, la base de datos, que se implementa en `IUserRepository` en este ejemplo, almacena una `LastChanged` valor.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="6a1ac-333">Cuando cualquier usuario se actualiza en la base de datos, la `LastChanged` valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="6a1ac-334">Con el fin de invalidar una cookie cuando los cambios de la base de datos se basan en el `LastChanged` valor, cree la cookie con un `LastChanged` notificación que contiene el actual `LastChanged` valor de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a1ac-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a1ac-336">Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="6a1ac-336">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="6a1ac-337">Un ejemplo es similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-337">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="6a1ac-338">Registrar la instancia de eventos durante el registro de servicio de cookie en el `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-338">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="6a1ac-339">Proporcionar un registro de servicio con ámbito para su `CustomCookieAuthenticationEvents` clase:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-339">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a1ac-340">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-340">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a1ac-341">Para implementar una invalidación para el `ValidateAsync` eventos, escribir un método con la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-341">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="6a1ac-342">ASP.NET Core Identity implementa esta comprobación como parte de su [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="6a1ac-342">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="6a1ac-343">Un ejemplo es similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-343">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="6a1ac-344">Registrar el evento durante la configuración de autenticación de cookie en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="6a1ac-344">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="6a1ac-345">Considere la posibilidad de una situación en la que se actualiza el nombre del usuario &mdash; una decisión que no afectan a la seguridad de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-345">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="6a1ac-346">Si desea actualizar la entidad de seguridad de usuario de manera no destructiva, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-346">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="6a1ac-347">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-347">The approach described here is triggered on every request.</span></span> <span data-ttu-id="6a1ac-348">Esto puede dar lugar a una reducción del rendimiento de gran tamaño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-348">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="6a1ac-349">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="6a1ac-349">Persistent cookies</span></span>

<span data-ttu-id="6a1ac-350">Puede que desee la cookie que se conservan entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-350">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="6a1ac-351">Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-351">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="6a1ac-352">El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres de explorador.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-352">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="6a1ac-353">Se respetan los parámetros de expiración deslizante configurados previamente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-353">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="6a1ac-354">Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie de una vez que se reinicie.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-354">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a1ac-355">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-355">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="6a1ac-356">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) clase reside en el `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-356">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a1ac-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="6a1ac-358">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) clase reside en el `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-358">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="6a1ac-359">Expiración de cookie absoluta</span><span class="sxs-lookup"><span data-stu-id="6a1ac-359">Absolute cookie expiration</span></span>

<span data-ttu-id="6a1ac-360">Puede establecer un tiempo de expiración absoluta con `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-360">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="6a1ac-361">También debe establecer `IsPersistent`; en caso contrario, `ExpiresUtc` se omite y se crea una cookie de sesión único.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-361">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="6a1ac-362">Cuando `ExpiresUtc` está establecido en `SignInAsync`, invalida el valor de la `ExpireTimeSpan` opción de `CookieAuthenticationOptions`, si se establece.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-362">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="6a1ac-363">El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una validez de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-363">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="6a1ac-364">Esto omite cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="6a1ac-364">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a1ac-365">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-365">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a1ac-366">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a1ac-366">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a><span data-ttu-id="6a1ac-367">Vea también</span><span class="sxs-lookup"><span data-stu-id="6a1ac-367">See also</span></span>

* [<span data-ttu-id="6a1ac-368">Cambios de autenticación 2.0 / anuncio de migración</span><span class="sxs-lookup"><span data-stu-id="6a1ac-368">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="6a1ac-369">Limitación de identidad por esquema</span><span class="sxs-lookup"><span data-stu-id="6a1ac-369">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)