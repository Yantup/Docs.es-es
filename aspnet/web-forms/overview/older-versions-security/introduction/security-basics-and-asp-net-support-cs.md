---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Conceptos básicos de seguridad y compatibilidad de ASP.NET (C#) | Microsoft Docs
author: rick-anderson
description: Este es el primer tutorial de una serie de tutoriales que se explora las técnicas para autenticar a los visitantes a través de un formulario web Forms, autorizar el acceso a personaliz...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 817a823ab5b09ea8bf2620a142779e79f53d957d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838728"
---
<a name="security-basics-and-aspnet-support-c"></a>Conceptos básicos de seguridad y compatibilidad de ASP.NET (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Este es el primer tutorial de una serie de tutoriales que se explora las técnicas para autenticar a los visitantes a través de un formulario web Forms, autorizar el acceso a determinadas páginas y la funcionalidad y administrar cuentas de usuario en una aplicación ASP.NET.


## <a name="introduction"></a>Introducción

¿Qué es la cosa que los foros, sitios de comercio electrónico, sitios Web de correo electrónico en línea, sitios Web del portal y los sitios de red social tienen en común? Todos ofrecen *cuentas de usuario*. Sitios que ofrecen las cuentas de usuario deben proporcionar un número de servicios. Como mínimo, nuevos visitantes tienen que poder crear una cuenta y visitantes que regresan deben ser capaces de iniciar sesión. Estas aplicaciones web pueden tomar decisiones basadas en el usuario que ha iniciado sesión: algunas páginas o acciones podrían estar restringidas solo para usuarios registrados o a un determinado subconjunto de usuarios; otras páginas podrían mostrar información específica para el usuario que ha iniciado sesión, o pueden mostrar más o menos información en función de qué usuario está viendo la página.

Este es el primer tutorial de una serie de tutoriales que se explora las técnicas para autenticar a los visitantes a través de un formulario web Forms, autorizar el acceso a determinadas páginas y la funcionalidad y administrar cuentas de usuario en una aplicación ASP.NET. En el transcurso de estos tutoriales, examinaremos cómo:

- Identificar y registrar usuarios en un sitio Web
- Use "ASP.NET's Membership framework" para administrar las cuentas de usuario
- Crear, actualizar y eliminar cuentas de usuario
- Limitar el acceso a una página web, el directorio o la funcionalidad específica según el usuario ha iniciado sesión
- Use "ASP.NET's Roles framework" para asociar cuentas de usuario a roles
- Administrar roles de usuario
- Limitar el acceso a una página web, el directorio o la funcionalidad específica según el rol del usuario que ha iniciado sesión
- Personalizar y ampliar "ASP.NET's security Web controls"

Estos tutoriales están pensados para ser concisos y se proveer instrucciones paso a paso con una gran cantidad de capturas de pantalla que le guiarán a través del proceso visualmente. Cada tutorial está disponible en versiones de Visual Basic y C# e incluye la descarga de código completo usado. (Este primer tutorial se centra en los conceptos de seguridad desde un punto de vista de alto nivel y, por tanto, no contiene ningún código asociado).

En este tutorial, trataremos los conceptos de seguridad importantes y qué funciones están disponibles en ASP.NET para ayudar en la implementación de roles, las cuentas de usuario, autorización y autenticación de formularios. Comencemos.

> [!NOTE]
> La seguridad es un aspecto importante de cualquier aplicación que abarca físico, tecnológico y las decisiones de directiva y requiere un alto grado de conocimientos de planeamiento y dominio. Esta serie de tutoriales no está pensada como una guía para el desarrollo de aplicaciones web seguras. En su lugar, se centra específicamente en roles, las cuentas de usuario, autorización y autenticación de formularios. Algunos conceptos de seguridad que pueda girar en torno a estos problemas se tratan en esta serie, mientras que otros usuarios se quedan sin explorar.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Autenticación, autorización, las cuentas de usuario y funciones

Autenticación, autorización, cuentas de usuario y roles son cuatro términos que se usará con frecuencia a lo largo de esta serie de tutoriales, por lo que me gustaría dedique un momento para definir estos términos dentro del contexto de seguridad web. En un modelo cliente / servidor, como Internet, hay muchos escenarios en los que el servidor necesita identificar al cliente que realiza la solicitud. *Autenticación* es el proceso de determinar la identidad del cliente. Un cliente que se ha identificado correctamente esta *autenticado*. Un cliente no identificado esta *no autenticado* o *anónimo*.

Los sistemas de autenticación segura involucran al menos una de las tres facetas siguientes: [algo que usted sabe, algo que tenga o algo que usted es](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). La mayoría de las aplicaciones web se basan en algo que el cliente conoce, como una contraseña o un PIN. La información utilizada para identificar a un usuario:  por ejemplo, su nombre de usuario y contraseña: son conocidas como *credenciales*. Esta serie de tutoriales se centra en *autenticación mediante formularios*, que es un modelo de autenticación donde los usuarios inician sesión en el sitio al proporcionar sus credenciales en un formulario de la página web. Todos hemos experimentado este tipo de autenticación antes. Vaya a cualquier sitio de comercio electrónico. Cuando esté listo para comprar deberá iniciar sesión escribiendo su nombre de usuario y contraseña en los cuadros de texto en una página web.

Además de identificar los clientes, un servidor que deba limitar qué recursos o funcionalidades son accesibles en función del cliente que realiza la solicitud. *Autorización* es el proceso de determinar si un usuario determinado tiene la autoridad para tener acceso a un recurso específico o funcionalidad.

Un *cuenta de usuario* es un almacén para guardar información acerca de un usuario determinado. Las cuentas de usuario como mínimo deben incluir información que identifica el usuario, como el nombre de inicio de sesión del usuario y contraseña. Junto con esta información esencial, cuentas de usuario pueden incluir cosas como: dirección de correo electrónico del usuario; la fecha y hora que se creó la cuenta; la fecha y hora, se registran por última vez nombre y apellidos; número de teléfono; y la dirección de correo electrónico. Cuando se usa la autenticación de formularios, normalmente se almacena la información de la cuenta de usuario en una base de datos relacional, como Microsoft SQL Server.

Aplicaciones Web que admiten las cuentas de usuario, opcionalmente, pueden agrupar usuarios en *roles*. Un rol es simplemente una etiqueta que se aplica a un usuario y proporciona una abstracción para definir las reglas de autorización y la funcionalidad de nivel de página. Por ejemplo, un sitio Web puede incluir un rol de administrador con reglas de autorización que prohíben cualquiera que no sea un administrador para tener acceso a un conjunto determinado de páginas web. Además, una variedad de páginas que son accesibles para todos los usuarios (incluidos los usuarios no administradores) podría presentar datos adicionales u ofrecen funcionalidad adicional cuando visita por los usuarios del rol de administradores. Uso de roles, podemos definir estas reglas de autorización en un rol por rol en lugar de por usuario.

## <a name="authenticating-users-in-an-aspnet-application"></a>Autenticación de usuarios en una aplicación ASP.NET

Cuando un usuario escribe una dirección URL en la ventana de dirección de su navegador o los clics en un vínculo, el explorador realiza una [protocolo de transferencia de hipertexto (HTTP)](http://en.wikipedia.org/wiki/HTTP) solicitar al servidor web para el contenido especificado, ya sea ASP.NET página, una imagen, JavaScript archivo, o cualquier otro tipo de contenido. El servidor web se encarga de devolver el contenido solicitado. Al hacerlo, debe determinar un número de cosas acerca de la solicitud, incluido quién realizó la solicitud y si la identidad está autorizada para recuperar el contenido solicitado.

De forma predeterminada, los exploradores envían las solicitudes HTTP que carecen de algún tipo de información de identificación. Pero si el explorador incluye información de autenticación, a continuación, el servidor web inicia el flujo de trabajo de autenticación, que intenta identificar al cliente que realiza la solicitud. Los pasos del flujo de trabajo de autenticación dependen del tipo de autenticación utilizado por la aplicación web. ASP.NET admite tres tipos de autenticación: formularios, Passport y Windows. Esta serie de tutoriales se centra en la autenticación de formularios, pero Dediquemos un minuto para comparar y contrastar el flujo de trabajo y los almacenes de usuario de autenticación de Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticación mediante la autenticación de Windows

El flujo de trabajo de autenticación de Windows usa una de las técnicas de autenticación siguientes:

- Autenticación básica
- Autenticación implícita
- Autenticación integrada de Windows

Las tres técnicas funcionan en aproximadamente la misma manera: cuando un no autorizado, llega una solicitud anónima, el servidor web devuelve una respuesta HTTP que indica que autorización es necesaria para continuar. El explorador, a continuación, muestra un cuadro de diálogo modal que pide al usuario su nombre de usuario y contraseña (consulte la figura 1). Esta información se envía al servidor web a través de un encabezado HTTP.


![Cuadro de diálogo Modal solicita al usuario sus credenciales](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figura 1**: un cuadro de diálogo Modal solicita al usuario sus credenciales


Se validan las credenciales proporcionadas con Store de usuario de Windows del servidor web. Esto significa que cada usuario autenticado en la aplicación web debe tener una cuenta de Windows en su organización. Esto es muy común en escenarios de intranet. De hecho, cuando se usa la autenticación integrada de Windows en una configuración de intranet, el explorador proporciona automáticamente el servidor web con las credenciales usadas para iniciar sesión en la red, con lo que se suprime el cuadro de diálogo que se muestra en la figura 1. Mientras la autenticación de Windows es excelente para aplicaciones de intranet, es imposible normalmente para aplicaciones de Internet ya que no desea crear las cuentas de Windows para cada usuario que se registra en el sitio.

### <a name="authentication-via-forms-authentication"></a>Autenticación mediante la autenticación de formularios

Autenticación de formularios, por otro lado, es ideal para aplicaciones web de Internet. Recuerde que la autenticación de formularios identifica el usuario de forma que les solicita que escriba sus credenciales a través de un formulario web. Por lo tanto, cuando un usuario intenta tener acceso a un recurso no autorizado, se redirigen automáticamente a la página de inicio de sesión donde puede escribir sus credenciales. A continuación, se validan las credenciales enviadas en un almacén de usuario personalizada - normalmente una base de datos.

Después de comprobar las credenciales enviadas, un *vale de autenticación de formularios* se crea para el usuario. Este vale indica que el usuario se ha autenticado e incluye información de identificación, como el nombre de usuario. El vale de autenticación de formularios (normalmente) se almacena como una cookie en el equipo cliente. Por lo tanto, visitas posteriores al sitio Web incluyen el vale de autenticación de formularios en la solicitud HTTP, lo que permite la aplicación web identificar al usuario una vez que inicies sesión.

Figura 2 se ilustra el flujo de trabajo de autenticación de formularios desde un mirador de alto nivel. Tenga en cuenta cómo actúan las piezas de autenticación y autorización en ASP.NET como dos entidades independientes. El sistema de autenticación de formularios identifica al usuario (o informa de que son anónimos). El sistema de autorización es lo que determina si el usuario tiene acceso al recurso solicitado. Si el usuario no está autorizado (tal como están en la figura 2 al intentar visitar anónimamente ProtectedPage.aspx), el sistema de autorización informa de que el usuario se deniega, causando los formularios de sistema de autenticación para redirigir automáticamente al usuario a la página de inicio de sesión.

Una vez que el usuario ha iniciado sesión correctamente, las solicitudes HTTP incluyen el vale de autenticación de formularios. El sistema de autenticación de formularios simplemente identifica al usuario: es el sistema de autorización que determina si el usuario puede acceder al recurso solicitado.


![El flujo de trabajo de autenticación de formularios](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figura 2**: el flujo de trabajo de autenticación de formularios


Profundizar en la autenticación de formularios con mucho más detalle en los próximos dos tutoriales,[una visión general de autenticación mediante formularios](an-overview-of-forms-authentication-cs.md) y [configuración de autenticación de formularios y temas avanzados](forms-authentication-configuration-and-advanced-topics-cs.md). Para obtener más información sobre ASP. Opciones de autenticación de la red, consulte [autenticación ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitar el acceso a las páginas Web, directorios y funcionalidad de la página

ASP.NET incluye dos métodos para determinar si un usuario determinado tiene autoridad para tener acceso a un determinado archivo o directorio:

- **Autorización de archivo** : puesto que las páginas ASP.NET y servicios web se implementan como archivos que residen en el sistema de archivos del servidor web, acceso a estos archivos se pueden especificar a través de listas de Control de acceso (ACL). Autorización de archivo se utiliza con más frecuencia con la autenticación de Windows porque las ACL son los permisos que se aplican a las cuentas de Windows. Cuando se usa la autenticación de formularios, la misma cuenta de Windows, independientemente del usuario que visite el sitio ejecuta todas las solicitudes de nivel de sistema operativo system - y archivo.
- **Autorización de URL**-con autorización de URL, el desarrollador de páginas especifica las reglas de autorización en Web.config. Estas reglas de autorización especifican qué usuarios o roles tienen acceso o se deniegan el acceso a ciertas páginas o los directorios de la aplicación.

Autorización de archivo y la autorización de URL definen reglas de autorización para tener acceso a una página ASP.NET determinada o para todas las páginas ASP.NET en un directorio determinado. Con estas técnicas nos podemos indicar a ASP.NET para denegar las solicitudes a una página concreta de un usuario determinado, o permitir el acceso a un conjunto de usuarios y denegar el acceso a todos los demás. ¿Qué sucede con escenarios donde todos los usuarios pueden acceder a la página, pero la funcionalidad de la página depende del usuario? Por ejemplo, muchos sitios que admiten las cuentas de usuario tienen páginas que muestran contenido diferente o datos para los usuarios autenticados frente a los usuarios anónimos. Un usuario anónimo podría ver un vínculo para iniciar sesión en el sitio, mientras que un usuario autenticado en su lugar, vería un mensaje, como, Bienvenido de nuevo, *Username* junto con un vínculo para cerrar sesión. Otro ejemplo: cuando esté viendo un elemento en un sitio de subastas vea información diferente dependiendo de que sea un usuario o la organización de subastas el elemento.

Estos ajustes de nivel de página pueden realizarse de forma declarativa o mediante programación. Para mostrar contenido diferente para anónimo a los usuarios autenticados, simplemente arrastre un [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) hasta la página y escriba el contenido adecuado a sus plantillas AnonymousTemplate y LoggedInTemplate. Como alternativa, se pueden determinar mediante programación si se autentica la solicitud actual, quién es el usuario y los roles que pertenecen a (si existe). Puede usar esta información para, a continuación, mostrar u ocultar columnas en una cuadrícula o paneles en la página.

Esta serie incluye tres tutoriales que se centran en la autorización. ***Autorización basada en usuario***examina cómo limitar el acceso a una página o páginas en un directorio para las cuentas de usuario específico; ***Autorización basado en el rol*** examina proporcionando las reglas de autorización en el rol de nivel; por último, el ***mostrar contenido en función del actualmente ha iniciado en usuario*** tutorial explora la modificación de un determinado la página contenido y funcionalidad según el usuario visita la página. Para obtener más información sobre ASP. Opciones de autorización de la red, consulte [autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Cuentas de usuario y funciones

ASP. Autenticación de formularios de la red proporciona una infraestructura para que los usuarios iniciar sesión en un sitio y tienen su estado autenticado recuerdan entre visitas de la página. Y la autorización de URL ofrece un marco para limitar el acceso a determinados archivos o carpetas en una aplicación ASP.NET. Ninguna de estas características, sin embargo, proporciona un medio para almacenar información de cuentas de usuario o administrar roles.

Antes de ASP.NET 2.0, los desarrolladores eran responsables de crear sus propios almacenes de usuario y el rol. También estaban en el enlace para diseñar las interfaces de usuario y escribir el código de usuario esencial páginas relacionadas con la cuenta, como la página de inicio de sesión y la página para crear una nueva cuenta, entre otros. Sin cualquier marco de la cuenta de usuario integrada en ASP.NET, cada desarrollador implementar cuentas de usuario tenían que llegan a sus propias decisiones de diseño en cuanto a preguntas como: ¿cómo se almacenan las contraseñas u otra información confidencial? ¿y qué instrucciones debo imponer con respecto a la intensidad y la longitud de contraseña?

Hoy en día, implementar cuentas de usuario en una aplicación de ASP.NET es mucho más sencillos gracias a la *marco de pertenencia* y controles Web de inicio de sesión integrado. El marco de pertenencia es una serie de clases en el [espacio de nombres System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) que proporciona funcionalidad para realizar tareas relacionadas con la cuenta de usuario esenciales. La clase clave en el marco de pertenencia es el [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que tiene métodos, como:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Usa el marco de pertenencia del [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa claramente los API del marco de pertenencia de su implementación. Esto permite a los desarrolladores usar una API común, pero permite a los que usen una implementación que satisfaga las necesidades de su aplicación personalizada. En resumen, la clase de pertenencia define la funcionalidad esencial de framework (métodos, propiedades y eventos), pero no proporciona realmente ningún detalle de implementación. En su lugar, los métodos de la clase de pertenencia al invocan el proveedor configurado, que es lo que realiza el trabajo real. Por ejemplo, cuando se invoca el método CreateUser de la clase de pertenencia, la clase de pertenencia no conoce los detalles del almacén de usuario. No sabe si los usuarios se mantienen en una base de datos o en un archivo XML o en otro almacén. La clase de pertenencia examina la configuración de la aplicación web para determinar qué proveedor delegar la llamada a, y esa clase de proveedor es responsable de crear realmente la nueva cuenta de usuario en el almacén de usuario adecuado. Esta interacción se ilustra en la figura 3.

Microsoft incluye dos clases de proveedor de pertenencia en .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementa la API de pertenencia en los servidores de Active Directory y Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementa la API de pertenencia en una base de datos de SQL Server.

Esta serie de tutoriales se centra exclusivamente en SqlMembershipProvider.


[![El modelo permite que diferentes implementaciones del proveedor siendo perfectamente conectado en el marco de trabajo&lt;/ seguro&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figura 03**: el modelo permite que diferentes implementaciones del proveedor siendo perfectamente conectado en el marco de trabajo ([haga clic aquí para ver imagen en tamaño completo](security-basics-and-asp-net-support-cs/_static/image5.png))


La ventaja del modelo de proveedor es que implementaciones alternativas pueden desarrolladas por Microsoft, proveedores o desarrolladores individuales y conectadas a la perfección en el marco de pertenencia. Por ejemplo, Microsoft ha lanzado [un proveedor de pertenencia para bases de datos de Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Para obtener más información sobre los proveedores de pertenencia, consulte el [Kit de herramientas de proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx), que incluye un tutorial de los proveedores de pertenencia, los proveedores personalizados de ejemplo, más de 100 páginas de documentación en el modelo de proveedor y el Complete el código fuente para los proveedores de pertenencia integrados (es decir, ActiveDirectoryMembershipProvider y SqlMembershipProvider).

ASP.NET 2.0 introdujo también el marco de trabajo de Roles. Al igual que el marco de pertenencia, el marco de trabajo de Roles se basa en el modelo de proveedor. Su API se expone a través de la [clase Roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) y .NET Framework se incluye con tres clases de proveedor:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -administra información de funciones en un almacén de directivas del Administrador de autorización, como Active Directory o ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementa funciones en una base de datos de SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -asocia información de funciones basada en grupo de Windows del visitante. Este método se utiliza normalmente con la autenticación de Windows.

Esta serie de tutoriales se centra exclusivamente en SqlRoleProvider.

Dado que el modelo de proveedor incluye una sola API hacia delante (las clases de pertenencia y Roles), es posible compilar funcionalidad en torno a esa API sin tener que preocuparse sobre los detalles de implementación: los que se controlan mediante los proveedores seleccionados por la página para desarrolladores. Esta API unificada permite a Microsoft y los proveedores de terceros compilar los controles Web que se comunican con los marcos de pertenencia y funciones. ASP.NET incluye una serie de [controla el inicio de sesión Web](https://msdn.microsoft.com/library/ms178329.aspx) para implementar interfaces de usuario de cuenta de usuario comunes. Por ejemplo, el [control Login](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) pide al usuario sus credenciales, la valida y, a continuación, los registra en mediante la autenticación de formularios. El [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) ofrece plantillas para mostrar un marcado diferente a los usuarios anónimos frente a los usuarios autenticados o un marcado diferente según el rol del usuario. Y el [control CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) proporciona una interfaz de usuario paso a paso para crear una nueva cuenta de usuario.

Interiormente los distintos controles de inicio de sesión interactúan con los marcos de pertenencia y funciones. La mayoría de los controles de inicio de sesión se pueden implementar sin tener que escribir una sola línea de código. Examinaremos estos controles con más detalle en próximos tutoriales, incluidas técnicas para extender y personalizar su funcionalidad.

## <a name="summary"></a>Resumen

Todas las aplicaciones web que admiten las cuentas de usuario requieren características similares, como: la capacidad de los usuarios iniciar sesión y tener su registro en estado recuerdan entre visitas de la página; una página web para los visitantes nuevo crear una cuenta; y la capacidad para el desarrollador de páginas para especificar qué recursos, los datos y funcionalidades están disponibles para qué usuarios o roles. Las tareas de autenticar y autorizar a los usuarios y de administración de roles y cuentas de usuario es sorprendentemente fácil de conseguir en aplicaciones ASP.NET gracias a la autenticación de formularios, autorización URL y los marcos de pertenencia y funciones.

En el transcurso de la siguientes varios tutoriales, examinaremos estos aspectos mediante la creación de una aplicación web de trabajo desde el principio de un modo paso a paso. En el tutorial en los dos próximos exploramos detalladamente la autenticación de formularios. Vemos el flujo de trabajo de autenticación de formularios en acción, analicemos el vale de autenticación de formularios, analizar problemas de seguridad y vea cómo configurar el sistema de autenticación de formularios: todo ello mientras compila una aplicación web que permite a los visitantes a iniciar sesión y cierre de sesión.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ASP.NET 2.0 Membership, Roles, autenticación de formularios y recursos de seguridad](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Directrices de seguridad 2.0 de ASP.NET](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticación de ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Introducción a los controles de inicio de sesión de ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examen de ASP.NET del 2.0 perfil, Roles y pertenencia](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [¿Cómo proteger mi sitio mediante la pertenencia y Roles I:?](https://asp.net/learn/videos/video-45.aspx) (Vídeo)
- [Introducción a la suscripción](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centro para desarrolladores de seguridad de MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 seguridad, la pertenencia y administración de roles](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Kit de herramientas de proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era este tutorial serie ha sido revisada por muchos revisores útiles. Los revisores para este tutorial incluyen Alicja Maziarz, John Suru y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](an-overview-of-forms-authentication-cs.md)
