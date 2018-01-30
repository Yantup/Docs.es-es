---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: "Pasar datos a páginas de vista maestra (C#) | Documentos de Microsoft"
author: microsoft
description: "El objetivo de este tutorial es explicar cómo se puede pasar los datos de un controlador a una página maestra de la vista. Se examinan dos estrategias para pasar datos a una vista m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b8bc8ce0690d2e45877be75011d8883facbc74a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="passing-data-to-view-master-pages-c"></a>Pasar datos a las páginas de vista maestra (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> El objetivo de este tutorial es explicar cómo se puede pasar los datos de un controlador a una página maestra de la vista. Se examinan dos estrategias para pasar datos a una página maestra de la vista. En primer lugar, se describe una solución sencilla que tiene como resultado una aplicación que es difícil de mantener. A continuación, examinaremos una solución mucho mejor que requiere un poco más trabajo inicial pero los resultados en una aplicación mucho más fácil de mantener.


## <a name="passing-data-to-view-master-pages"></a>Pasar datos a páginas principales de vista

El objetivo de este tutorial es explicar cómo se puede pasar los datos de un controlador a una página maestra de la vista. Se examinan dos estrategias para pasar datos a una página maestra de la vista. En primer lugar, se describe una solución sencilla que tiene como resultado una aplicación que es difícil de mantener. A continuación, examinaremos una solución mucho mejor que requiere un poco más trabajo inicial pero los resultados en una aplicación mucho más fácil de mantener.

### <a name="the-problem"></a>El problema

Imagine que está creando una aplicación de base de datos de la película y desea que aparezca la lista de categorías de la película en todas las páginas de la aplicación (consulte la figura 1). Además, imagine que la lista de categorías de la película se almacena en una tabla de base de datos. En ese caso, tendría sentido para recuperar las categorías de la base de datos y presentar la lista de categorías de película dentro de una página maestra de la vista.


[![Mostrar categorías de la película en una página maestra de vista](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Figura 01**: Mostrar categorías de la película en una página maestra de vista ([haga clic aquí para ver la imagen a tamaño completo](passing-data-to-view-master-pages-cs/_static/image3.png))


Este es el problema. ¿Cómo recuperar la lista de categorías de la película en la página maestra? Es tentador para llamar a métodos de las clases de modelo en la página maestra directamente. En otras palabras, es tentador para incluir el código para recuperar los datos de la derecha de la base de datos en la página maestra. Sin embargo, omitiendo los controladores MVC para tener acceso a la base de datos podría infringir la separación clara de intereses que es una de las principales ventajas de la creación de una aplicación MVC.

En una aplicación MVC, desea que todas las interacciones entre las vistas MVC y el modelo MVC controlando los controladores MVC. Esta separación de intereses da como resultado una aplicación más fácil de mantener, adaptable y pueden someterse a prueba.

En una aplicación MVC, todos los datos que se pasan a una vista, incluida una página maestra de la vista, deben pasarse a una vista por una acción de controlador. Además, los datos deben pasarse aprovechando las ventajas de los datos de vista. En el resto de este tutorial, examinar los dos métodos de pasar los datos de vista a una página maestra de la vista.

### <a name="the-simple-solution"></a>La solución Simple

Comenzaremos con la solución más sencilla para pasar datos de vista de un controlador a una página maestra de la vista. La solución más sencilla consiste en pasar los datos de vista de la página maestra en cada acción de controlador.

Tenga en cuenta el controlador en la lista 1. Expone dos acciones denominadas `Index()` y `Details()`. El `Index()` método de acción devuelve cada película en la tabla de base de datos de películas. El `Details()` método de acción devuelve cada película en una categoría determinada de película.

**Lista 1:`Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Tenga en cuenta que la Index() y las acciones de Details() agregan dos elementos para ver los datos. La acción de Index() agrega dos claves: categorías y películas. La clave categorías representa la lista de categorías de la película aparece en la página principal de la vista. La clave de películas representa la lista de películas que se muestran en la página de vista de índice.

La acción Details() también agrega dos claves con el nombre de categorías y películas. La clave de categorías, una vez más, representa la lista de categorías de la película aparece en la página principal de la vista. La clave de películas representa la lista de películas en una categoría determinada que se muestra en la página de vista de detalles (consulte la figura 2).


[![La vista de detalles](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Figura 02**: vista de detalles de la ([haga clic aquí para ver la imagen a tamaño completo](passing-data-to-view-master-pages-cs/_static/image6.png))


La vista de índice se encuentra en el listado 2. Simplemente recorre en iteración la lista de películas representada por el elemento de películas en los datos de vista.

**La lista 2:`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

La vista de página maestra se encuentra en la lista 3. La página maestra de vista recorre en iteración y representa todas las categorías de película representadas por el elemento de categorías de datos de la vista.

**Enumerar 3:`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Todos los datos se pasan a la vista y la página maestra de vista a través de los datos de vista. Que es la forma correcta para pasar datos a la página maestra.

¿Por lo tanto, cuál es el problema con esta solución? El problema es que esta solución infringe el principio de SECA (no repitas). Cada acción del controlador debe agregar la misma muy lista de categorías de película para ver los datos. Tener código duplicado en la aplicación hace que la aplicación sea mucho más difícil de mantener, adaptar y modificar.

### <a name="the-good-solution"></a>La solución correcta

En esta sección, examinaremos una solución alternativa y, a continuación, mejor, para pasar datos de una acción de controlador a una página maestra de la vista. En lugar de agregar las categorías de película de la página maestra en cada acción de controlador, agregamos las categorías de película a los datos de vista solo una vez. Todos los datos de vista utilizados por la página maestra de la vista se agrega en un controlador de la aplicación.

La clase ApplicationController se encuentra en el listado 4.

**Enumerar 4:`Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Hay tres cosas que debe tener en cuenta acerca del controlador de aplicación en el listado 4. En primer lugar, tenga en cuenta que la clase hereda de la clase System.Web.Mvc.Controller base. El controlador de la aplicación es una clase de controlador.

En segundo lugar, tenga en cuenta que la clase de controlador de aplicación es una clase abstracta. Una clase abstracta es una clase que debe ser implementada por una clase concreta. Dado que el controlador de la aplicación es una clase abstracta, no no se puede invocar los métodos definidos en la clase directamente. Si se intenta invocar directamente la clase de la aplicación obtendrá un mensaje de error no se encuentra el recurso.

En tercer lugar, tenga en cuenta que el controlador de aplicación contiene un constructor que agrega la lista de categorías de película para ver los datos. Cada clase de controlador que se hereda desde el controlador de la aplicación llama automáticamente a constructor del controlador de la aplicación. Cada vez que se llama a cualquier acción en cualquier controlador que se hereda desde el controlador de la aplicación, las categorías de la película se incluye automáticamente en los datos de vista.

El controlador de películas en el listado 5 se hereda desde el controlador de la aplicación.

**Enumerar 5:`Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

El controlador de películas, al igual que el controlador Home descrito en la sección anterior, expone dos métodos de acción denominados `Index()` y `Details()`. Tenga en cuenta que la lista de categorías de la película aparece en la página principal de vista no es agregado a ver datos en el `Index()` o `Details()` método. Puesto que el controlador de películas hereda desde el controlador de la aplicación, se agrega la lista de categorías de película para ver los datos automáticamente.

Tenga en cuenta que esta solución para agregar datos de vista de una página maestra de vista no infringe el principio de SECA (no repitas). El código para agregar la lista de categorías de película para ver los datos se encuentra en una única ubicación: el constructor para el controlador de la aplicación.

### <a name="summary"></a>Resumen

En este tutorial, se describen dos métodos para pasar datos de vista de un controlador a una página maestra de la vista. En primer lugar, se examina un sencillo, pero es difícil mantener el enfoque. En la primera sección, explicamos cómo puede agregar datos de la vista de una página maestra de la vista en cada acción de cada controlador de la aplicación. Se concluye que ésta fue un enfoque incorrecto porque infringe el principio de SECA (no repitas).

A continuación, se examina una mejor estrategia para agregar los datos requeridos por una página maestra de la vista para ver los datos. En lugar de agregar los datos de vista en cada acción de controlador, hemos agregado una sola vez los datos de la vista dentro de un controlador de la aplicación. De este modo, puede evitar código duplicado cuando se pasan datos a una página maestra de la vista en una aplicación ASP.NET MVC.

>[!div class="step-by-step"]
[Anterior](creating-page-layouts-with-view-master-pages-cs.md)
[Siguiente](asp-net-mvc-views-overview-vb.md)