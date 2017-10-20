# Listado 

## Parametros de código

AL momento de programar, es necesario seguir una serie de parámatros y
estándares para que todos podamos entender de que va el código cuando tomamos
un proyecto que ha realizado otra persona.
Esta sección estará dividida en grupos, los cuales son Código fuente, bases de
datos.

  ### 1. **Código Fuente.**

Es importante mantener estándares en el código fuente para facilicar la
escalabilidad, legibilidad y mantenimiento de las aplicaciones.

  * **Clases**: Los nombres de clase deben ser sustantivos en Mayúscula CamelCase, con la primera letra de cada palabra en mayúscula. Use palabras completas, evitar acrónimos y abreviaturas (a menos que la abreviatura es mucho más extendido que el de la forma larga, como la dirección URL o HTML). Ejemplo `class Raster`, `class User`

  * **Métodos**: Los métodos deben ser verbos en minúscula CamelCase o un conjunto de varias palabras que comienza con un verbo en minúsculas; es decir, con la primera letra en minúscula y la primera letra de las palabras siguientes en mayúsculas. Ejemplo `walk()`, `walkInTheBeach()`, `getSun()`

  * **Variables**: Las variables locales, variables de instancia y variables de clase también se escriben en minúscula CamelCase. Los nombres de variable no deben comenzar con los caracteres guión bajo ( _ ) o el signo de dólar "$" (a menos que estemos trabajando en php en cuyo caso es extrictamente necesario). Esto contrasta con otras convenciones de codificación que establecen que los guiones deben ser usados como prefijo todas las variables de instancia. Los nombres de variables deben ser cortos pero significativos. La elección de un nombre de variable debe ser [mnemónico](https://es.wikipedia.org/wiki/Regla_mnemot%C3%A9cnica), es decir, diseñado para indicar al observado la intención de su uso. Se deben evitar los nombres de variables de un solo carácter excepto para las variables temporales. Los nombres comunes de las variables temporales son i, j, k, m, y n para enteros; c, d, y e para los caracteres. Ejemplo `int i`, `char c`, `float myWidth`, `$myHeight`

  * **Constantes**: Los nombres de las constantes se deben escribir en mayúsculas separadas por guiones bajos. Los nombres de constantes pueden contener dígitos, pero no como el primer carácter. Ejemplo `int MAX_HEIGHT = 10`, `const PI = 3.14`

  * **Documentar**: Es estrictamente necesario documentar todos los métodos/funciones y las variables de clase de la siguiente forma:


  - **Documentando Métodos**:

```C#
/*
* Compute the sum of two integers
*
* @param {int} a
* @param {int} b
*
* @return {int}
*
*/
    public static int sum(int a, int b){
      
      return a + b;
    }
```


```javascript
/*
* Compute the subtraction of two numbers usingsubtractsubstract javascript
*
* @param {number} a
* @param {number} b
*
* @return {number}
*
*/
    function subtractsubstract(a, b){
      
      return a - b;
    }subtractsubstract
```

  - **Documentanto Variables**:

```php
/**
* The name of table to use.
*
* @var {string}
*/
protected $table = 'users';

```

  ### 2. **Paradigmas de diseño.**

  * **Modelo Vista Controlador**: 
  La arquitectura a implementar debe ser del tipo Modelo-Vista-Controlador, este es un patrón de arquitectura de software, que separa los datos y la lógica de negocio de una aplicación de la interfaz de usuario y el módulo encargado de gestionar los eventos y las comunicaciones. Para ello MVC propone la construcción de tres componentes distintos que son el modelo, la vista y el controlador, es decir, por un lado define componentes para la representación de la información, y por otro lado para la interacción del usuario Para saber más  sobre este patrón de diseño visita [(MVC)](https://es.wikipedia.org/wiki/Modelo%E2%80%93vista%E2%80%93controlador). Acá también debemos manejar convenciones de nombres

    1. **Modelos**: Debe ser un sustantivo en mayúscula CamelCase, debe seguir la misma
    convención que se mencionó al momento de crear clases. Ejemplo `User.php`, `Role.cs`, `Permission.js`

    2. **Controladores**: Debe ser la misma convencione que para el modelo pero debe
    contener al final la palabra Controller. Ejemplo `UserController.php`, `RoleController.cs`, `PermissionController.js`

    3. **Vistas**: las vistas debe comenzar con el nombre del modelo principal a usar o un sustantivo en minúscula camelCase y especificar la accion que realiza. Elemplo `userEdit.blade.php`, `userCreate.cshtml`  



  * API:
  Los sistemas que requiera conexiones a traves de API deben seguir el modelo  Transferencia de Estado Representacional [(Rest)](https://es.wikipedia.org/wiki/Transferencia_de_Estado_Representacional) en formato JSON o XML

  Los nombres para los entry points de las api deben ser de la forma `/prefijoControlador/acción/identificador` y en caso de que se pertiman los grupos de entry
  points deben implementarse Ejemplo 


```php
Route::group(['prefix' => 'users'], function(){

  route::post('/create', 'UserController@store'); 
  route::put('/update/{user_id}', 'UserController@update'); 

});
```


```C#
public class MvcApplication : System.Web.HttpApplication{
    public static void RegisterRoutes(RouteCollection routes){
        routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

        routes.MapRoute(
            "Default",                                              // Route name 
            "{controllerPrefix}/{action}/{id}",                           // URL with parameters 
            new { controller = "User", action = "create", id = "" }  // Parameter defaults
        );

    }

    protected void Application_Start(){
        RegisterRoutes(RouteTable.Routes);
    }
}
```

  2. Bases de datos (DB)

  * Al momento de usar DB realacionales (Mysql, MsSql server, etc) se debe seguir ciertos parámetros para los nombres de tabla, estos deben estar basados en los modelos del paradigma MVC. El nombre debe ser un sustantivo en minúsculas estilo snake_case separados con guión bajo ( _ ) . Ejemplo, al tener el modelo `User` la tabla debe llamarse `users` en plural, al contrario del modelo que debe ser singular. Si por el contrario es una tabla que representa una relación entre dos modelos, el nombre debe estar basados en el modelo [entidad relacion](https://es.wikipedia.org/wiki/Modelo_entidad-relaci%C3%B3n) Ejemplo `user_roles`, `role_permissions`

  * Todas las tablas deben contener un campo `id`, entero autoincrementable sin signo, un campo `created_at` del tipo timestamp y un campo `updated_at` también del tipo timestamp. 
  
  * Las tablas que requeran relacionarse deben contener sus [Claves foráneas](https://es.wikipedia.org/wiki/Clave_for%C3%A1nea) relacionando de esta manera los campos de  las tablas que pertenezcan a la relación. Esto es importante porque mantiene la integridad de los datos y además, ya que las claves foráneas debe ser indices, acelera los tiempos de consulta

```SQL
CREATE TABLE orders (
    id int UNSIGNED AUTO_INCREMENT NOT NULL,
    order_number int NOT NULL,
    person_id int,
    PRIMARY KEY (id),
    FOREIGN KEY (person_id) REFERENCES persons(id)
);
```

  Ejemplo usando el ORM Eloquent 

```php
public function up()
{
  Schema::create('role_user', function (Blueprint $table) {
    $table->increments('id');
    $table->integer('role_id')->unsigned()->index();
    $table->integer('user_id')->unsigned()->index();
    $table->timestamps();

    $table->foreign('role_id')->references('id')->on('roles')->onDelete('cascade');
    $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
  });
}
```
