---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Crear un modelo de datos más compleja para una aplicación ASP.NET MVC (4 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: beda9585a3050e35c250d0cb7ab1dc8a464efa3d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816248"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Crear un modelo de datos más compleja para una aplicación ASP.NET MVC (4 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En los tutoriales anteriores, ha trabajado con un modelo de datos simples que se componía de tres entidades. En este tutorial agregará más entidades y relaciones, y personalizará el modelo de datos mediante la especificación de formato, validación y las reglas de asignación de base de datos. Verá dos formas de personalizar el modelo de datos: al agregar atributos a clases de entidad y agregando código a la clase de contexto de base de datos.

Cuando haya terminado, las clases de entidad conformarán el modelo de datos completo que se muestra en la ilustración siguiente:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizar el modelo de datos mediante el uso de atributos

En esta sección verá cómo personalizar el modelo de datos mediante el uso de atributos que especifican reglas de formato, validación y asignación de base de datos. A continuación, en algunas de las secciones siguientes creará la completa `School` modelo de datos mediante la adición de atributos a las clases ya creadas y crear nuevas clases para los restantes tipos de entidad en el modelo.

### <a name="the-datatype-attribute"></a>El atributo de tipo de datos

Para las fechas de inscripción de estudiantes, en todas las páginas web se muestra actualmente la hora junto con la fecha, aunque todo lo que le interesa para este campo es la fecha. Mediante los atributos de anotación de datos, puede realizar un cambio de código que fijará el formato de presentación en cada vista en la que se muestren los datos. Para ver un ejemplo de cómo hacerlo, deberá agregar un atributo a la propiedad `EnrollmentDate` en la clase `Student`.

En *Models\Student.cs*, agregue un `using` instrucción para el `System.ComponentModel.DataAnnotations` espacio de nombres y agregue `DataType` y `DisplayFormat` atributos a la `EnrollmentDate` propiedad, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

El [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo se usa para especificar un tipo de datos que es más específico que el tipo intrínseco de base de datos. En este caso solo se quiere realizar el seguimiento de la fecha, no de la fecha y la hora. El [enumeración DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) proporciona muchos tipos de datos, como *fecha, hora, PhoneNumber, Currency, EmailAddress* y mucho más. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo, un `mailto:` vínculo puede crearse para [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), y se puede proporcionar un selector de fecha para [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) en exploradores que admiten [HTML5](http://html5.org/). El [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emite de atributos HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (pronunciado *"datos dash"*) los atributos que los exploradores HTML 5 pueden comprender. El [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos no proporcionan ninguna validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De forma predeterminada, se muestra el campo de datos según los formatos predeterminados basados en el servidor [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


El `ApplyFormatInEditMode` configuración especifica que el formato especificado también se debe aplicar cuando el valor se muestra en un cuadro de texto para su edición. (No es posible que desee que algunos campos, por ejemplo, para los valores de moneda, es posible que no desea el símbolo de moneda en el cuadro de texto para su edición.)

Puede usar el [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo por sí solo, pero suele ser una buena idea usar el [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo también. El `DataType` atributo transmite la *semántica* de los datos como contraposición a cómo se representan en una pantalla y proporciona las siguientes ventajas que no conseguimos `DisplayFormat`:

- El explorador puede habilitar características de HTML5 (por ejemplo mostrar un control de calendario, el símbolo de divisa adecuado según la configuración regional, vínculos de correo electrónico, etcetera.).
- De forma predeterminada, el explorador representa los datos con el formato correcto en función de su [configuración regional](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- El [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo puede habilitar MVC elegir la plantilla de campo a la derecha para representar los datos (el [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) si utiliza por sí mismo utiliza la plantilla de cadena). Para obtener más información, consulte Brad Wilson [plantillas de ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Aunque se escribió para MVC 2, en este artículo todavía se aplica a la versión actual de ASP.NET MVC.)

Si usas el `DataType` atributo con un campo de fecha, tendrá que especificar el `DisplayFormat` atributo también con el fin de asegurarse de que el campo se representa correctamente en exploradores de Chrome. Para obtener más información, consulte [este hilo](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Vuelva a ejecutar la página de índice de Student y tenga en cuenta que ya no se muestran los tiempos de las fechas de inscripción. El mismo es cierto para cualquier vista que usa el `Student` modelo.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

También puede especificar reglas de validación de datos y mensajes mediante atributos. Imagine que quiere asegurarse de que los usuarios no escriban más de 50 caracteres para un nombre. Para agregar esta limitación, agregue [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributos a la `LastName` y `FirstMidName` propiedades, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

El [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo no impedir que un usuario escriba espacios en blanco para un nombre. Puede usar el [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo para aplicar restricciones a la entrada. Por ejemplo, el código siguiente requiere el primer carácter en mayúsculas y el resto de caracteres sean alfabéticos:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

El [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atributo proporciona una funcionalidad similar a la [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo pero no proporciona el cliente validación.

Ejecute la aplicación y haga clic en el **estudiantes** ficha. Muestra el siguiente error:

*El modelo que respalda el contexto 'SchoolContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

El modelo de base de datos ha cambiado de forma que requiere un cambio en el esquema de base de datos y Entity Framework detectó. Usará las migraciones para actualizar el esquema sin perder los datos que ha agregado a la base de datos mediante el uso de la interfaz de usuario. Si cambia los datos que se creó mediante la `Seed` método, que se cambiarán a su estado original debido la [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) método que se usa en el `Seed` método. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) es equivalente a una operación "upsert" de la terminología de la base de datos.)

En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

El `add-migration MaxLengthOnNames` comando crea un archivo denominado  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Este archivo contiene código que se actualizará la base de datos para que coincida con el modelo de datos actual. La marca de tiempo que se antepone al nombre de archivo de migraciones se usa por Entity Framework para ordenar las migraciones. Después de haber creado varias migraciones, si coloca la base de datos, o si se implementa el proyecto mediante el uso de las migraciones, todas las migraciones se aplican en el orden en el que se crearon.

Ejecute el **crear** página y escriba cualquier nombre de más de 50 caracteres. Tan pronto como superar los 50 caracteres, validación del lado cliente muestra inmediatamente un mensaje de error.

![error de val lado del cliente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>El atributo de columna

También puede usar atributos para controlar cómo se asignan las clases y propiedades a la base de datos. Imagine que hubiera usado el nombre `FirstMidName` para el nombre de campo por la posibilidad de que el campo contenga también un segundo nombre. Pero quiere que la columna de base de datos se denomine `FirstName`, ya que los usuarios que van a escribir consultas ad hoc en la base de datos están acostumbrados a ese nombre. Para realizar esta asignación, puede usar el atributo `Column`.

El atributo `Column` especifica que, cuando se cree la base de datos, la columna de la tabla `Student` que se asigna a la propiedad `FirstMidName` se denominará `FirstName`. En otras palabras, cuando el código hace referencia a `Student.FirstMidName`, los datos procederán o se actualizarán en la columna `FirstName` de la tabla `Student`. Si no especifica nombres de columna, obtienen el mismo nombre que el nombre de propiedad.

Agregue una mediante declaración para [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) y el atributo de nombre de columna en la `FirstMidName` propiedad, como se muestra en el siguiente código resaltado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

La adición de la [atributo de columna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) cambia el modelo de seguridad de la SchoolContext, por lo que no coincidirá con la base de datos. Escriba los siguientes comandos en la PMC para crear otra migración:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

En **Explorador de servidores** (**Database Explorer** si utiliza Express para Web), haga doble clic en el *estudiante* tabla.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

La siguiente imagen muestra el nombre de columna original, tal como estaba antes de aplicar las dos primeras migraciones. Además del nombre de columna cambia de `FirstMidName` a `FirstName`, las dos columnas de nombre han cambiado desde `MAX` longitud de 50 caracteres.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Base de datos también se puede realizar cambios de asignación mediante el [API Fluent](https://msdn.microsoft.com/data/jj591617), como verá más adelante en este tutorial.

> [!NOTE]
> Si intenta compilar antes de finalizar la creación de todas estas clases de entidad, podría obtener errores del compilador.


## <a name="create-the-instructor-entity"></a>Crear la entidad Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Crear *Models\Instructor.cs*, reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Tenga en cuenta que varias propiedades son las mismas en las entidades `Student` y `Instructor`. En el [implementación de la herencia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie, deberá refactorizar utilizando la herencia para eliminar esta redundancia.

### <a name="the-required-and-display-attributes"></a>La información necesaria y atributos de presentación

Los atributos en el `LastName` propiedad especificar que se trata de un campo obligatorio, ya que el título del cuadro de texto debe ser "Last Name" (en lugar del nombre de propiedad, que sería "LastName" sin espacio), y que el valor no puede tener más de 50 caracteres.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

El [atributo StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) establece la longitud máxima de la base de datos y proporciona el cliente y servidor validación de ASP.NET MVC. En este atributo también se puede especificar la longitud mínima de la cadena, pero el valor mínimo no influye en el esquema de la base de datos. El [atributo requerido](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) no es necesaria para tipos de valor como DateTime, int, double y float. Tipos de valor no se puede asignar un valor null, por lo que son intrínsecamente necesarios. Puede quitar el [atributo requerido](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) y reemplazarlo por un parámetro de longitud mínima para el `StringLength` atributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Puede colocar varios atributos en una sola línea, por lo que también podría escribir la clase instructor como sigue:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>La propiedad calculada FullName

`FullName` es una propiedad calculada que devuelve un valor que se crea mediante la concatenación de otras dos propiedades. Por lo tanto, solo tiene un `get` descriptor de acceso y no `FullName` se generará una columna en la base de datos.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Los cursos y las propiedades de navegación OfficeAssignment

`Courses` y `OfficeAssignment` son propiedades de navegación. Tal como se explicó anteriormente, se definen normalmente como [virtual](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) por lo que puede aprovechar una característica de Entity Framework denominada [la carga diferida](https://msdn.microsoft.com/magazine/hh205756.aspx). Además, si una propiedad de navegación puede contener varias entidades, su tipo debe implementar la [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) interfaz. (Por ejemplo [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) pero no se consideran [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) porque `IEnumerable<T>` no implementa [agregar ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un instructor puede impartir cualquier número de cursos, por lo que `Courses` se define como una colección de `Course` entidades. Estado de nuestras reglas de negocio un instructor solo puede tener a lo sumo una oficina, por lo que `OfficeAssignment` se define como una sola `OfficeAssignment` entity (lo que puede ser `null` si se asigna ninguna oficina).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Crear la entidad OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Crear *Models\OfficeAssignment.cs* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compile el proyecto, que guarda los cambios y comprueba que no ha realizado ninguna copia y pegue el compilador puede detectar los errores.

### <a name="the-key-attribute"></a>El atributo clave

Hay una relación uno a cero o uno entre el `Instructor` y `OfficeAssignment` entidades. Solo existe una asignación de oficina en relación con el instructor está asignado a y, por lo tanto, su clave principal también es su clave externa para el `Instructor` entidad. Pero Entity Framework no reconoce automáticamente `InstructorID` como principal clave de esta entidad porque su nombre no sigue el `ID` o *classname* `ID` convención de nomenclatura. Por tanto, se usa el atributo `Key` para identificarla como la clave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

También puede usar el `Key` atributo si la entidad tiene su propia clave principal, pero desea que la propiedad de nombre algo distinto a `classnameID` o `ID`. De forma predeterminada EF trata la clave como no generada por la base de datos porque la columna es para una relación de identificación.

### <a name="the-foreignkey-attribute"></a>El atributo ForeignKey

Cuando hay una relación uno a cero o uno o una relación uno a uno entre dos entidades (por ejemplo, entre `OfficeAssignment` y `Instructor`), EF no puede averiguar qué extremo de la relación es la entidad de seguridad y qué extremo es dependiente. Relaciones uno a uno tienen una propiedad de navegación de referencia en cada clase a la otra clase. El [ForeignKey atributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) se pueden aplicar a la clase dependiente para establecer la relación. Si se omite el [ForeignKey atributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), obtendrá el siguiente error al intentar crear la migración:

No se puede determinar el extremo principal de una asociación entre los tipos 'ContosoUniversity.Models.OfficeAssignment' y 'ContosoUniversity.Models.Instructor'. El extremo principal de esta asociación debe configurarse explícitamente mediante la API fluida de relación o las anotaciones de datos.

Más adelante en el tutorial le mostraremos cómo configurar esta relación con la API fluida.

### <a name="the-instructor-navigation-property"></a>La propiedad de navegación de Instructor

El `Instructor` entidad tiene una que acepta valores NULL `OfficeAssignment` propiedad de navegación (porque un instructor podría no tener una asignación de oficina) y el `OfficeAssignment` entidad tiene un distinto de NULL `Instructor` propiedad de navegación (debido a una asignación de oficina no puede existir sin un instructor-- `InstructorID` es distinto de null). Cuando un `Instructor` entidad tiene un relacionados `OfficeAssignment` entidad, cada entidad tendrá una referencia a la otra en su propiedad de navegación.

Podría poner un `[Required]` atributo en la propiedad de navegación de Instructor para especificar que debe haber un instructor relacionado, pero no tiene que hacer eso, porque la clave externa de InstructorID (que también es la clave para esta tabla) es distinto de NULL.

## <a name="modify-the-course-entity"></a>Modificar la entidad Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

En *Models\Course.cs*, reemplace el código que agregó anteriormente con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

La entidad course tiene una propiedad de clave externa `DepartmentID` que apunta a la que se relaciona `Department` entidad y que tiene un `Department` propiedad de navegación. Entity Framework no requiere que agregue una propiedad de clave externa al modelo de datos cuando tenga una propiedad de navegación para una entidad relacionada. EF crea automáticamente claves externas en la base de datos siempre que se necesiten. Pero tener la clave externa en el modelo de datos puede hacer que las actualizaciones sean más sencillas y eficaces. Por ejemplo, cuando se recuperar una entidad course para modificarla, la `Department` entity es null si no la carga, así que cuando se actualiza la entidad course, deberá capturar primero la `Department` entidad. Cuando la propiedad de clave externa `DepartmentID` se incluye en el modelo de datos, no es necesario capturar el `Department` entidad antes de actualizar.

### <a name="the-databasegenerated-attribute"></a>El atributo DatabaseGenerated

El [atributo DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con el [ninguno](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parámetro en el `CourseID` propiedad especifica que los valores de clave principales son proporcionados por el usuario, en lugar de generados por la base de datos.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

De forma predeterminada, Entity Framework se da por supuesto que la base de datos genera valores de clave principal. Es lo que le interesa en la mayoría de los escenarios. Sin embargo, para `Course` entidades, usará un número de curso especificado por el usuario como una serie 1000 para un departamento, una serie 2000 para otro departamento y así sucesivamente.

### <a name="foreign-key-and-navigation-properties"></a>Clave externa y las propiedades de navegación

Las propiedades de clave externa y propiedades de navegación de la `Course` entidad reflejan las relaciones siguientes:

- Un curso se asigna a un departamento, por lo que hay una clave externa `DepartmentID` y una propiedad de navegación `Department` por las razones mencionadas anteriormente. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un curso puede tener cualquier número de alumnos inscritos en él, por lo que la propiedad de navegación `Enrollments` es una colección: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un curso puede ser impartido por varios instructores, por lo que la propiedad de navegación `Instructors` es una colección: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Creación de la entidad Department

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Crear *Models\Department.cs* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>El atributo de columna

Anteriormente usó el [atributo de columna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) para cambiar la asignación de nombre de columna. En el código para el `Department` entidad, el `Column` atributo que se va a se usa para cambiar SQL asignar tipos de datos para que la columna se definirá mediante SQL Server [dinero](https://msdn.microsoft.com/library/ms179882.aspx) tipo en la base de datos:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Asignación de columnas generalmente no es necesaria, dado que Entity Framework normalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR que defina para la propiedad. El tipo CLR `decimal` se asigna a un tipo `decimal` de SQL Server. Pero en este caso, sabe que la columna va a contener cantidades de moneda y el [dinero](https://msdn.microsoft.com/library/ms179882.aspx) es más adecuado para ese tipo de datos.

### <a name="foreign-key-and-navigation-properties"></a>Clave externa y las propiedades de navegación

Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:

- Un departamento puede tener o no un administrador, y un administrador es siempre un instructor. Por lo tanto, el `InstructorID` propiedad se incluye como la clave externa para el `Instructor` se agrega la entidad y un signo de interrogación después de la `int` designación para marcar la propiedad que acepta valores NULL del tipo. La propiedad de navegación se denomina `Administrator` pero contiene un `Instructor` entidad: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un departamento puede tener varios cursos, por lo que hay un `Courses` propiedad de navegación: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Por convención, Entity Framework permite la eliminación en cascada para las claves externas que no aceptan valores NULL y para las relaciones de varios a varios. Esto puede dar lugar a reglas de eliminación en cascada circular, lo que provocarán una excepción cuando se ejecuta el código de inicializador. Por ejemplo, si no define el `Department.InstructorID` propiedad que acepta valores NULL, se obtendría el siguiente mensaje de excepción cuando se ejecuta el inicializador: "la relación referencial dará como resultado una referencia cíclica no permitida." Si es necesario de las reglas de negocio `InstructorID` propiedad que no acepta valores NULL, tendría que usar la siguiente API fluida para deshabilitar eliminación en cascada en la relación: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Modificación de la entidad Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

En *Models\Student.cs*, reemplace el código que agregó anteriormente con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>La entidad Enrollment

 En *Models\Enrollment.cs*, reemplace el código que agregó anteriormente con el código siguiente

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Clave externa y las propiedades de navegación

Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:

- Un registro de inscripción es para un solo curso, por lo que hay una propiedad de clave externa `CourseID` y una propiedad de navegación `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Un registro de inscripción es para un solo estudiante, por lo que hay una propiedad de clave externa `StudentID` y una propiedad de navegación `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relaciones Varios a Varios

Hay una relación de varios a varios entre la `Student` y `Course` entidades y el `Enrollment` entidad funciona como una tabla de combinación de varios a varios *con carga* en la base de datos. Esto significa que el `Enrollment` tabla contiene datos adicionales, además de las claves externas para las tablas combinadas (en este caso, una clave principal y un `Grade` propiedad).

En la ilustración siguiente se muestra el aspecto de estas relaciones en un diagrama de entidades. (Este diagrama se ha generado mediante la [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); crear el diagrama no forma parte del tutorial, simplemente se usa aquí como una ilustración.)

![Many_relationship Course_many de alumno](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, que indica una relación uno a varios.

Si el `Enrollment` tabla no incluye información de nivel, solo tendría que contener las dos claves externas `CourseID` y `StudentID`. En ese caso, corresponde a una tabla combinada-to-many *sin carga* (o un *tabla combinada pura*) en la base de datos, y no tiene que crear una clase de modelo para él en absoluto. El `Instructor` y `Course` las entidades tienen ese tipo de relación de varios a varios, y como puede ver, no hay ninguna clase de entidad entre ellos:

![Many_relationship Course_many de instructor](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Se requiere una tabla combinada en la base de datos, sin embargo, como se muestra en el siguiente diagrama de base de datos:

![Many_relationship_tables Course_many de instructor](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework crea automáticamente el `CourseInstructor` tabla y leer y actualizarla indirectamente por leer y actualizar el `Instructor.Courses` y `Course.Instructors` las propiedades de navegación.

## <a name="entity-diagram-showing-relationships"></a>Diagrama de entidades en el que se muestran las relaciones

En la siguiente ilustración se muestra el diagrama creado por Entity Framework Power Tools para el modelo School completado.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Además de las líneas de relación de varios a varios (\* a \*) y las líneas de relación de uno a varios (1 a \*), puede ver aquí la línea de relación de uno a cero o uno (1 a 0.. 1) entre el `Instructor` y `OfficeAssignment` las entidades y la línea de relación de cero-o-uno a varios (0.. 1 a \*) entre las entidades Instructor y departamento.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizar el modelo de datos mediante la adición de código en el contexto de base de datos

A continuación, agregará las nuevas entidades para el `SchoolContext` clase y personalizar algunas de la asignación mediante [API fluida](https://msdn.microsoft.com/data/jj591617) llamadas. (La API es "fluida" porque a menudo se usa para encadenar una serie de llamadas de método juntos en una sola instrucción).

En este tutorial usará la API fluida para la asignación de la base de datos que no se puede realizar con atributos. Pero también se puede usar la API fluida para especificar casi todas las reglas de formato, validación y asignación que se pueden realizar mediante el uso de atributos. Algunos atributos como `MinimumLength` no se pueden aplicar con la API fluida. Como se mencionó anteriormente, `MinimumLength` no cambia el esquema, sólo se aplica una regla de validación del lado cliente y servidor

Algunos desarrolladores prefieren usar la API fluida exclusivamente para mantener "limpias" las clases de entidad. Si quiere, puede mezclar atributos y la API fluida, y hay algunas personalizaciones que solo se pueden realizar mediante la API fluida, pero en general el procedimiento recomendado es elegir uno de estos dos enfoques y usarlo de forma constante siempre que sea posible.

Para agregar las nuevas entidades a los datos de modelo y realizan la asignación de la base de datos que no ha hecho mediante el uso de atributos, reemplace el código de *DAL\SchoolContext.cs* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nueva instrucción en el [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método configura la tabla de combinación de varios a varios:

- Para la relación de varios a varios entre la `Instructor` y `Course` entidades, el código especifica los nombres de tabla y columna de la tabla de combinación. Código primero puede configurar la relación de varios a varios que sin este código, pero si no lo llame, obtendrá los nombres predeterminados, como `InstructorInstructorID` para el `InstructorID` columna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

El código siguiente proporciona un ejemplo de cómo se podría haber usado la API fluida en lugar de atributos para especificar la relación entre el `Instructor` y `OfficeAssignment` entidades:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Para obtener información sobre qué hacen las instrucciones "API fluida" en segundo plano, vea el [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) entrada de blog.

## <a name="seed-the-database-with-test-data"></a>Inicialización de la base de datos con datos de prueba

Reemplace el código en el *migrations\configuration* archivo con el código siguiente con el fin de proporcionar datos de inicialización para las nuevas entidades que ha creado.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Como se vio en el primer tutorial, la mayor parte de este código simplemente actualiza o crea objetos de entidad y carga de datos de ejemplo en las propiedades según sea necesario para las pruebas. Sin embargo, tenga en cuenta cómo el `Course` entidad, que tiene una relación varios a varios con el `Instructor` entidad, se controla:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Cuando creas un `Course` objeto, inicializa el `Instructors` propiedad de navegación como una colección vacía con el código `Instructors = new List<Instructor>()`. Esto hace posible agregar `Instructor` entidades que están relacionados con esto `Course` utilizando el `Instructors.Add` método. Si no creó una lista vacía, no podrá agregar estas relaciones, porque el `Instructors` propiedad sería null y no tendríamos un `Add` método. También puede agregar la inicialización de lista al constructor.

## <a name="add-a-migration-and-update-the-database"></a>Agregar una migración y actualizar la base de datos

En la PMC, escriba el `add-migration` comando:

`PM> add-Migration Chap4`

Si se intenta actualizar la base de datos en este momento, obtendrá el siguiente error:

*La instrucción ALTER TABLE en conflicto con la restricción FOREIGN KEY "FK\_dbo. Curso\_dbo. Departamento\_DepartmentID ". Se produjo el conflicto en la tabla base de datos "contosouniversity" por "dbo. Department", columna"DepartmentID".*

Editar el &lt; *timestamp&gt;\_Chap4.cs* de archivos y realice los siguientes cambios de código (agregará una instrucción SQL y modificar un `AddColumn` instrucción):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Asegúrese de que se marque como comentario o eliminación las existentes `AddColumn` línea cuando se agrega una nueva o, obtendrá un error al escribir el `update-database` comando.)

En ocasiones, al ejecutar migraciones con datos existentes, necesita insertar datos de código auxiliar en la base de datos para satisfacer las restricciones foreign key, y eso es lo que está haciendo ahora. El código generado agrega un distinto de NULL `DepartmentID` clave externa para el `Course` tabla. Si ya hay filas en el `Course` tabla cuando se ejecuta el código, la `AddColumn` operación produciría un error porque SQL Server no sabe qué valor para incluir en la columna que no puede ser null. Por lo tanto, ha cambiado el código para asignar un valor predeterminado a la nueva columna, y ha creado un departamento de código auxiliar denominado "Temp" para que actúe como el departamento predeterminado. Como resultado, si existen `Course` filas cuando se ejecuta este código, estarán todos relacionados con el departamento "Temp".

Cuando el `Seed` ejecuciones de método, va a insertar filas en el `Department` tabla y relacionan existente `Course` filas a esas nuevas `Department` filas. Si no ha agregado ningún curso en la interfaz de usuario, a continuación, ya no necesitaría el departamento "Temp" o el valor predeterminado en la `Course.DepartmentID` columna. Para permitir la posibilidad de que alguien podría no agregó cursos mediante el uso de la aplicación, ¿desea actualizar el `Seed` código del método para asegurarse de que todos los `Course` filas (no solo los insertadas por las ejecuciones anteriores de la `Seed` método) tienen válido `DepartmentID` valores antes de quitar el valor predeterminado de la columna de valor y elimine el departamento "Temp".

Cuando haya terminado de editar el &lt; *timestamp&gt;\_Chap4.cs* de archivo, escriba el `update-database` comando en la PMC para ejecutar la migración.

> [!NOTE]
> Es posible generar otros errores al migrar datos y realizar cambios de esquema. Si se producen errores de migración que no se puede resolver, puede cambiar la cadena de conexión en el *Web.config* de archivos o eliminar la base de datos. El enfoque más sencillo consiste en cambiar el nombre de la base de datos en *Web.config* archivo. Por ejemplo, cambiar el nombre de la base de datos a CU\_probar tal y como se muestra en la siguiente:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  Con una base de datos, no hay ningún dato para migrar y el `update-database` comando es mucho más probable que se completará sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, consulte [cómo quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Abra la base de datos en **Explorador de servidores** como hizo anteriormente y expanda el **tablas** nodo para ver que se han creado todas las tablas. (Si sigue teniendo **Explorador de servidores** abierta desde el momento anterior, haga clic en el **actualizar** botón.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

No cree una clase de modelo para el `CourseInstructor` tabla. Como se explicó anteriormente, se trata de una tabla combinada para la relación de varios a varios entre la `Instructor` y `Course` entidades.

Haga clic en el `CourseInstructor` de tabla y seleccione **mostrar datos de tabla** para comprobar que tiene datos en ella como resultado de la `Instructor` las entidades que se ha agregado a la `Course.Instructors` propiedad de navegación.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Resumen

Ahora tiene un modelo de datos más complejo y la base de datos correspondiente. En el siguiente tutorial aprenderá más acerca de diferentes maneras de obtener acceso a datos relacionados.

Pueden encontrar vínculos a otros recursos de Entity Framework en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
