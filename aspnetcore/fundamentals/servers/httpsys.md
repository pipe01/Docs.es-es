---
title: Implementación del servidor web HTTP.sys en ASP.NET Core
author: guardrex
description: Obtenga información sobre HTTP.sys, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo kernel de HTTP.sys, HTTP.sys es una alternativa a Kestrel que se puede usar para conectarse directamente a Internet sin IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 125008e2168ec55ffdfa62f5c3ff44c66066424c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207074"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementación del servidor web HTTP.sys en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://github.com/guardrex)

> [!NOTE]
> Este tema solo se aplica a ASP.NET Core 2.0 o posterior. En versiones anteriores de ASP.NET Core, HTTP.sys se denominaba [WebListener](xref:fundamentals/servers/weblistener).

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) es un [servidor web de ASP.NET Core](xref:fundamentals/servers/index) que solo se ejecuta en Windows. HTTP.sys supone una alternativa a [Kestrel](xref:fundamentals/servers/kestrel) y ofrece algunas características que este no facilita.

> [!IMPORTANT]
> HTTP.sys no es compatible con el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y no se puede usar con IIS o IIS Express.

HTTP.sys admite las siguientes características:

* [Autenticación de Windows](xref:security/authentication/windowsauth)
* Uso compartido de puertos
* HTTPS con SNI
* HTTP/2 a través de TLS (Windows 10 o posterior)
* Transmisión directa de archivos
* Almacenamiento en caché de respuestas
* WebSockets (Windows 8 o posterior)

Versiones de Windows compatibles:

* Windows 7 o posterior
* Windows Server 2008 R2 o posterior

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Cuándo usar HTTP.sys

HTTP.sys resulta útil para implementaciones en las que:

* Es necesario exponer el servidor directamente a Internet sin usar IIS.

  ![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

* Una implementación interna requiere una característica que no está disponible en Kestrel, como [Autenticación de Windows](xref:security/authentication/windowsauth).

  ![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

HTTP.sys es una tecnología consolidada que protege contra muchos tipos de ataques y que proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características. El propio IIS se ejecuta como agente de escucha de HTTP sobre HTTP.sys.

## <a name="http2-support"></a>Compatibilidad con HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) está habilitado para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:

* Windows Server 2016/Windows 10 o posterior
* Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)
* Conexión con TLS 1.2 o una versión posterior

::: moniker range=">= aspnetcore-2.2"

Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/1.1`.

::: moniker-end

HTTP/2 está habilitado de forma predeterminada. Si no se establece una conexión HTTP/2, la conexión vuelve a HTTP/1.1. En una futura versión de Windows, las marcas de configuración de HTTP/2 estarán disponibles, incluida la capacidad para deshabilitar HTTP/2 con HTTP.sys.

## <a name="kernel-mode-authentication-with-kerberos"></a>Autenticación de modo kernel con Kerberos

HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos. La autenticación de modo usuario no se admite con Kerberos y HTTP.sys. Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario. Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.

## <a name="how-to-use-httpsys"></a>Cómo usar HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Configuración de la aplicación de ASP.NET Core para usar HTTP.sys

1. No se requiere una referencia de paquete en el archivo de proyecto cuando se usa el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 o posterior). Si no usa el metapaquete `Microsoft.AspNetCore.App`, agregue una referencia de paquete a [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Llame al método de extensión [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) extensión al compilar el host de web y especifique las [opciones de HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions) necesarias:

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   La configuración adicional de HTTP.sys se controla a través de [Configuración del Registro](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **Opciones de HTTP.sys**

   | Propiedad. | Descripción | Default |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Controlar si se permite la entrada/salida sincrónica de los objetos `HttpContext.Request.Body` y `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Permitir solicitudes anónimas. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Especificar los esquemas de autenticación permitidos. Puede modificarse en cualquier momento antes de eliminar el agente de escucha. Los valores se proporcionan con la [enumeración AuthenticationSchemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None` y `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Intentar el almacenamiento en memoria caché en [modo kernel](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) de las respuestas con encabezados elegibles. Es posible que la respuesta no incluya encabezados `Set-Cookie`, `Vary` o `Pragma`. Debe incluir un encabezado `Cache-Control` que sea `public` y un valor `shared-max-age` o `max-age`, o un encabezado `Expires`. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Número máximo de aceptaciones simultáneas. | Cinco &times; [entorno.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Establecer el número máximo de conexiones simultáneas que se aceptan. Use `-1` para infinito. Use `null` para usar la configuración de la máquina del Registro. | `null`<br>(ilimitado) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Vea la sección <a href="#maxrequestbodysize">MaxRequestBodySize</a>. | 30 000 000 bytes<br>(~28,6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Número máximo de solicitudes que se pueden poner en cola. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Indicar si las escrituras del cuerpo de respuesta que no se producen debido a desconexiones del cliente deben iniciar excepciones o finalizar con normalidad. | `false`<br>(finalizar con normalidad) |
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Exponer la configuración de [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) de HTTP.sys, que también puede configurarse en el Registro. Siga los vínculos de API para obtener más información sobre cada configuración, incluidos los valores predeterminados:<ul><li>[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; Tiempo permitido para que la API HTTP Server purgue el cuerpo de la entidad en una conexión persistente.</li><li>[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; Tiempo permitido para que llegue el cuerpo de la entidad de solicitud.</li><li>[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; Tiempo permitido para que la API HTTP Server analice el encabezado de solicitud.</li><li>[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; Tiempo permitido para una conexión inactiva.</li><li>[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; Tasa mínima de envío de la respuesta.</li><li>[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; Tiempo permitido para que la solicitud permanezca en la cola de solicitudes antes de que la aplicación la recoja.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Especifique el objeto [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) que se registra con HTTP.sys. El más útil es [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), que se usa para agregar un prefijo a la colección. Pueden modificarse en cualquier momento antes de eliminar el agente de escucha. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Tamaño máximo permitido de cualquier cuerpo de solicitud en bytes. Cuando se establece en `null`, el tamaño máximo del cuerpo de solicitud es ilimitado. Este límite no tiene ningún efecto en las conexiones actualizadas, que siempre son ilimitadas.

   El método recomendado para reemplazar el límite de una aplicación de ASP.NET Core MVC para un solo objeto `IActionResult` consiste en usar el atributo [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) en un método de acción:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Se inicia una excepción si la aplicación intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud. Puede usarse una propiedad `IsReadOnly` que indique si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.

   Si la aplicación debe reemplazar [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) por solicitud, use la interfaz [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. If usa Visual Studio, asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.

   En Visual Studio, el perfil de inicio predeterminado corresponde a IIS Express. Para ejecutar el proyecto como aplicación de consola, cambie manualmente el perfil seleccionado, tal y como se muestra en la siguiente captura de pantalla:

   ![Seleccionar el perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Configurar Windows Server

1. Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd), instale .NET Core, .NET Framework o ambos (si se trata de una aplicación de .NET Core que tiene como destino .NET Framework).

   * **.NET Core** &ndash; si la aplicación requiere .NET Core, obtenga y ejecute el instalador de .NET Core desde las [descargas de .NET](https://www.microsoft.com/net/download/all).
   * **.NET Framework** &ndash; Si la aplicación requiere .NET Framework, vea [.NET Framework: Guía de instalación](/dotnet/framework/install/) para encontrar las instrucciones de instalación. Instale la versión necesaria de .NET Framework. El instalador de la versión más reciente de .NET Framework se puede encontrar en las [descargas de .NET](https://www.microsoft.com/net/download/all).

2. Configure los puertos y las direcciones URL de la aplicación.

   ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada. Para configurar los puertos y los prefijos de dirección URL, las opciones incluyen el uso de:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * El argumento de la línea de comandos `urls`
   * La variable de entorno `ASPNETCORE_URLS`
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   En el ejemplo de código siguiente se muestra cómo se usa [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Una ventaja de `UrlPrefixes` es que se genera inmediatamente un mensaje de error para prefijos con formato incorrecto.

   La configuración de `UrlPrefixes` invalida la configuración de `UseUrls`/`urls`/`ASPNETCORE_URLS`. Por lo tanto, la ventaja de `UseUrls`, `urls` y la variable de entorno `ASPNETCORE_URLS` es que resulta más fácil de cambiar entre Kestrel y HTTP.sys. Para obtener más información sobre `UseUrls`, `urls` y `ASPNETCORE_URLS`, vea el tema [Hospedaje en ASP.NET Core](xref:fundamentals/host/index).

   HTTP.sys usa los [formatos de cadena UrlPrefix de la API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Los enlaces de carácter comodín de nivel superior (`http://*:80/` y `http://+:80`) **no** se deben usar. Los enlaces de carácter comodín de nivel superior pueden exponer su aplicación a vulnerabilidades de seguridad. Esto se aplica tanto a los caracteres comodín fuertes como a los débiles. Use nombres de host explícitos en lugar de caracteres comodín. Los enlaces de carácter comodín de subdominio (por ejemplo, `*.mysub.com`) no suponen este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable). Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.

3. Registre previamente los prefijos de dirección URL para enlazarse a HTTP.sys y configurar los certificados x.509.

   Si los prefijos de dirección URL no se han registrado previamente en Windows, ejecute la aplicación con privilegios de administrador. La única excepción se produce al enlazarse a localhost con HTTP (no HTTPS) con un número de puerto superior a 1024. En ese caso, no se requieren privilegios de administrador.

   1. La herramienta integrada para configurar HTTP.sys es *netsh.exe*. *netsh.exe* se usa para reservar prefijos de dirección URL y asignar certificados X.509. Esta herramienta requiere privilegios de administrador.

      En este ejemplo se muestran los comandos para reservar prefijos de dirección URL para los puertos 80 y 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      En este ejemplo se muestra cómo asignar un certificado X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Documentación de referencia de *netsh.exe*:

      * [Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
      * [Cadenas de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. Si es necesario, cree certificados X.509 autofirmados.

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. Abra puertos del firewall para permitir que el tráfico llegue a HTTP.sys. Use *netsh.exe* o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

En el caso de las aplicaciones hospedadas por HTTP.sys que interactúan con las solicitudes de Internet o de una red corporativa, puede que sea necesario configurar más elementos si esas aplicaciones se hospedan detrás de servidores proxy y equilibradores de carga. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Recursos adicionales

* [API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [Repositorio aspnet/HttpSysServer de GitHub (código fuente)](https://github.com/aspnet/HttpSysServer/)
* [Hospedaje en ASP.NET Core](xref:fundamentals/host/index)
