---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: '¿Cómo: crear una aplicación auxiliar HTML personalizada para una aplicación de MVC? | Microsoft Docs'
author: rick-anderson
description: En este vídeo, Chris Pels muestra cómo crear un HtmlHelper personalizado que no está disponible en el conjunto estándar de una aplicación MVC. En primer lugar, una aplicación MVC de ejemplo...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 4061c06cfeab2278e5732295b034f81f7995c2a4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838454"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a>¿Cómo: crear una aplicación auxiliar HTML personalizada para una aplicación de MVC?
====================
por [Chris Pels](https://twitter.com/chrispels)

En este vídeo, Chris Pels muestra cómo crear un HtmlHelper personalizado que no está disponible en el conjunto estándar de una aplicación MVC. En primer lugar, se crea una aplicación de MVC de ejemplo con un controlador de demostración y una vista para probar la HtmlHelper personalizado. A continuación, se crea un módulo con una función pública que sea un método de extensión que representa la implementación de la HtmlHelper personalizado. Es la aplicación auxiliar personalizada para crear `<img>` etiquetas en una página y recibe varios parámetros de entrada, incluidos el identificador, la dirección url y el texto alternativo para la etiqueta de imagen. La lógica, a continuación, se agrega a la función para devolver el completado `<img>` etiqueta con la información especificada. A continuación, se usa el HtmlHelper personalizado en la página de demostración para mostrar una imagen. Por último, el HtmlHelper personalizado se expande para incluir varias invalidaciones de constructor que proporcionan flexibilidad para obtener más fácilmente creando diferentes `<img>` etiquetas.

[&#9654;Vea el vídeo (18 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> [Anterior](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Siguiente](how-do-i-work-with-model-binders-in-an-mvc-application.md)
