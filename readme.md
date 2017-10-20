# Convenciones 

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
* Compute the subtraction of two numbers
*
* @param {number} a
* @param {number} b
*
* @return {number}
*
*/
    function subtract(a, b){
      
      return a - b;
    }
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



  * **REST.**
  Los sistemas que requiera conexiones a traves de API deben seguir el modelo  Transferencia de Estado Representacional [(Rest)](https://es.wikipedia.org/wiki/Transferencia_de_Estado_Representacional) en formato JSON preferentemente o XML si es estrictamente necesario

  Los nombres para los entry points o puntos de entrada de las api deben ser de la forma `/prefijoControlador/acción/identificador` y en caso de que se pertiman los grupos de entry points deben implementarse, los métodos Http a usar deben estar definidos como el paradigma lo dicta  `GET` para recuperar los datos, `POST` para crear datos nuevos, `PUT` para modificar datos existentes y `DELETE` para eliminar datos. Ejemplo 


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

  ## Bases de datos (DB).

  * Cuando se usen bases de datos relacionales (Mysql, MsSql server, etc) se debe seguir ciertos parámetros para los nombres de tabla, estos deben estar basados en los nombres de modelos del paradigma MVC. El nombre debe ser un sustantivo en minúsculas estilo snake_case separados con guión bajo ( _ ). Ejemplo, al tener el modelo `User` la tabla debe llamarse `users` en plural, al contrario del modelo que debe estar singular. Por otro lado en una tabla que represente una relación entre dos modelos, el nombre debe estar basados en el diagrama [entidad relación](https://es.wikipedia.org/wiki/Modelo_entidad-relaci%C3%B3n) de la base de datos. Ejemplo `user_roles`, `role_permissions`

  * Todas las tablas deben contener un campo `id`, entero autoincrementable sin signo, un campo `created_at` del tipo timestamp y un campo `updated_at` también del tipo timestamp, este campo debe ser actualizado con cada modificación que reciba la fila ú objeto. 
  
  * Los campos de relación deben llamarse de la forma (tabla a relacionar en singular) tablaarelacionarensingular_id ejemplo `delivery_id`.
  
  * Las tablas que requieran relacionarse deben contener sus respectivas [claves foráneas](https://es.wikipedia.org/wiki/Clave_for%C3%A1nea) estableciendo de esta manera los campos de las tablas que pertenezcan a la relación. Esto es importante porque mantiene la integridad de los datos y además, ya que las claves foráneas debe ser indices, disminuye los tiempos de consulta.
  
  
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


  * Si alguna tabla tiene relaciones opcionales, el campo de la relación debe establecerce como `null` y tener valor por defecto también `null`



```SQL
CREATE TABLE clients (
    id int UNSIGNED AUTO_INCREMENT NOT NULL,
    name varchar(40) NOT NULL,
    user_id int UNSIGNED NULL DEFAULT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```


  * **Estructuras de datos**

  Si se necesita crear estructuras de datos en formato JSON, trabajo muy habitual en javascript, este debe seguir las convenciones que se usan para bases de datos siempre y cuando solo contengan datos y no funciones/métodos, en cuyo caso, debe seguir la conveción para la creación de clases.

  * Datos.

```javascript
 {
  'name': 'jane doe',
  'email': 'jane@example.com',
  'phone_number': '1149593938'
 }
```

  * Pseudo objetos.


```javascript
 var User = {
  /*
  * the name
  *
  * @var {string}
  */
  name: 'jane doe',
  /*
  * return the name
  *
  * @return {string}
  */
  getName: function(){
    return this.name; 
  }

 }
```

* **Desarrollo**

* Test Drive Development.

  El proceso de desarrollo debe estar basado en el Test Drive Development [(TDD)](https://es.wikipedia.org/wiki/Desarrollo_guiado_por_pruebas) este habla sobre el desarrollo de pruebas unitarias o test unitarios para luego pasar a desarrollar los mecanismos para superar dichos test. Los test deben realizarse en al menos 4 casos escenciales

    1. Interacciones comunes del usuario con la aplicación.
    2. Casos de errores que esperas que sucedan frecuentemente (ejemplo que el usuario introduzca una contraseña errónea).
    3. Integraciones con servicios de terceros.
    4. Todas las funciones definidas públicas dentro de la aplicación.

* Control de versiones.

  El sistema de control de versiones a usar es [git](https://git-scm.com/), he implementaremos una modificación del flujo de trabajo llamado [gitflow](http://aprendegit.com/que-es-git-flow/)

   A continuación una breve explicación del flujo de trabajo con git.

  Cada cambio que se realice debera ser en un branch/rama nuevo (evitar hacer cambios sobre develop/master), el nombre del branch esta dado por el ticket asignado: `PRO-xx` donde xx es el número de ticket proveniente de Jira ó el sistema de tranqueo de tareas que se este usando y PRO es el prefijo definido para el proyecto.

  1- Antes de hacer el nuevo branch, se debe hacer un pull de la rama develop para trabajar con los últimos cambios: `git checkout develop`, seguido de `git pull`.

  2- Una vez actualizada la rama develop, hacer un branch nuevo a partir de este: `git checkout -b PRO-xx` (crea el branch PRO-xx a partir de develop y se mueve allí).

  3- Una vez que hemos trabajado en la rama y esten los cambios hechos hacer el commit, desde la raíz del prouyecto hacer: `git add .` ó `git add -A`, luego `git commit -m 'PRO-xx Descripcion que aparece en el ticket'`. 

  4- En caso de tener varios commits pertenecientes a una misma tarea o ticket, debemos hacer un squash para mantener un único commit por Branch. El squash junta x cantidad de commits en uno solo. Al hacer `git log` podemos ver la cantidad de commits que hicimos en ese branch (los que tienen como comentario PRO-xx del que estemos trabajando).

  5- Si NO tenemos más de un commit saltar este paso. En caso contrario, al tener más de un commit por tarea generamos el squash con `git rebase -i HEAD~xx` (donde xx es la cantidad de commits que generamos y podemos ver con git log). Aparece una pantalla listando en cada renglon los commits involucrados, cada uno empieza con la palabra pick. Debemos dejar el primer pick como esta y luego cambiar todos los demas por squash. guardamos y salimos. Se abre el editor con todos los comentarios de los distintos commits, acá debemos dejar uno solo (Por lo general el primero, el original). Nuevamente guardamos y salimos. Ya esta el squash listo, si hacemos `git log` podemos ver que solo figuara un commit de la branch (luego del commit de la branch aparece la historia de commits de donde viene).

  En caso de haber estado trabajando algunos días en la branch, deberemos actualizar la branch antes de hacer el push (para no generar conflictos).

  6- Traemos los últimos cambios a develop, para eso hacemos el paso 1.
  
  7- Volvemos a nuestra branch `git checkout PRO-xx` y hacemos un rebase `git rebase develop`.

  Una vez actualizado esta listo para subir los cambios y crear el Pull Request.

  8- Para hacer el push al servidor de la branch, hacemos: `git push -u origin PRO-xx`. Cuando hacemos el push nos aparece un link donde podemos acceder directamente para crear el Pull Request (PR) en el servidor de control de veriones (bitbucket, github, gitlab).
  
  9- Cuando creamos el PR no olvidarse: por defecto compara el branch generado para mergear con master, hay que cambiarlo para que lo compare con develop. Luego de revisar que esten los cambios deseados crear el PR.


  10- Una vez creado el Pull Request (PR) el miembro del equipo ó los miembros del equipo encargados del repositorio deberán hacer code review y si todo esta bien proceder a hacer el merge de esta rama en develop.

