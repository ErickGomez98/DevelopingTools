### Tutorial para combinar página web con Sistema interno, para que se pueda trabajar en los dos, en un mismo dominio con las mismas tabla. Ambos creados con drupal.

`Funcionando en drupal 7.56`
1. Hacer respaldo del sistema interno (junto con sus respectivas bases de datos)
2. Hacer respaldo de la página Web (junto con sus respectivas bases de datos)
3. Descargar la última versión del módulo [Themekey](https://www.drupal.org/project/themekey), e instalarlo en el drupal de la página web. Al momento de habilitar el módulo, solamente es necesario habilitar el "ThemeKey: Map themes to Drupal paths or object properties." Descargar e instalar la última versión del módulo [Login Destination](https://www.drupal.org/project/login_destination), y habilitar normalmente el módulo.
4. Hay que utilizar las tablas de usuarios y roles del sistema interno, tener en cuenta que hacer lo siguiente eliminará cualquier rol, permiso, y usuario creado en la página web, por lo que si existian mas usuarios/roles/permisos que no sean los que vienen por default en drupal, hay que crearlos en el sistema interno o hay que saber cuales son para volver a generarlos después de haber borrado las tablas, entonces, eliminar el contenido de las siguientes tablas de la base de datos de la página web:
    - role
    - role_permissions
    - users
    - users_roles
    - sessions
5. Una vez con las tablas vacias, hay que agarrar todo el contenido de las mismas tablas (excepto la de sessions), pero ahora del sistema interno, e insertarlas en las tablas correspondientes en la base de datos de la página Web. Una vez se tengan todos los datos traspasados, abrá que volver a iniciar sesión, hay que recordar que se borraron todos los usuarios anteriores, por lo que si olvidaste crear el usuario que usabas para administrar la página web, hay que entrar con el usuario de administrador de drupal del sistema interno y volver a crear todos los usuarios/permisos/roles necesarios.
6. Una vez con los módulos instalados en el paso 3, hay que crear un Rol para identificar a los usuarios que usarán el sistema interno, el nombre del Rol puede ser "Interno" o "Intranet" o cualquier otro siempre y cuando se identifique.
7. Asignar el Rol previamente creado a todos los usuarios que se requiera que los meta al sistema interno, en caso de que los usuarios no tengan este rol, nunca verán el sistema interno una vez que inicien sesión.
8. Identificar cuál es el tema padre del sistema interno (Normalmente se utiliza Bootstrap como tema principal.), se puede saber dentro de la carpeta de temas del drupal del sistema interno, o en la pestaña de apariencia en el drupal admin. 
9. Una vez se sepa cual es el tema padre del sistema interno, ver si será necesario meterlo al drupal de la página web (puede ser que no sea necesario, porque ya se tenia bootstrap instalado anteriormente, o puede ser que si sea necesario porque no se habia utilizado en la página web). En caso de ser necesario, solamente copiar la carpeta de del tema padre dentro de la carpeta de temas de la página web.
10. Ir a la carpeta del drupal del sistema interno, agarrar la carpeta del theme (que normalmente tiene el mismo nombre que el módulo o la página web: `/public_html/dev/sites/all/themes/~folder~`), y pegarla en la carpeta de los temas del drupal de la página web (`/public_html/dev/sites/all/themes/`)
11. Activar el tema del sistema interno dentro de `admin/appearance`.
> En caso de que el sistema interno este desarrollado en bootstrap 4, seguir el paso 10, de lo contrario, brincarse hasta el paso 13
12. Como el módulo del sistema interno esta desarrollado en Bootstrap 4, será necesario hacer varios ajustes para que funcione, ya que el theme bootstrap del drupal está en la versión 3. Para ello, realizar los [siguientes pasos](Configure_bootstrap_4_drupal.md), una vez terminados, regresar aquí y continuar.
13. Ir a `admin/appearance/settings/~theme_name~` y configurar correctamente el módulo en base a las configuraciones que tenia en el drupal del sistema interno, para ello solamente hay que irse a la misma ruta pero en el drupal del sistema interno, y copiar la configuración exactamente igual. 
14. Configurar el módulo Themekey:
    1. Ir a admin/config/user-interface/themekey
    2. Posicionarse en la pestaña de Theme Switching Rule Chain
    3. En la parte de abajo de esa vista, hay que agregar una nueva regla:
        - New Rule:  user:role  =  `nombre de Rol`, seleccionar el theme del sistema interno y marcar la casilla de "Enabled"  
    4. Guardar Configuración.
    - Con los pasos anteriores, se configuró el módulo del themekey para que hiciera lo siguiente:
        - Al momento de que se le ingresa una nueva regla condicionada por el Rol, el módulo va a trabajar en esa regla del rol de usuario, al momento de que el usuario inicie sesión, el themekey verificará si cuenta con una regla basada en ese rol, en caso de tenerla, cambiará el tema al interno.
15. Configurar el módulo Login Destination:
    1. Ir a admin/config/people/login-destination
    2. Click en Add login destination rule
    3. Seleccionar  Internal page or external URL e ingresar una url del sistema a redireccionar, normalmente suelen ser `ìntranet/...`
    4. Marcar Login, registration, one-time login link
    5. Seleccionar All pages except those listed
    6. En el apartado de Redirect users with roles, ahi seleccionar el rol que previamente se creó para identificar a los usuarios internos del sistema.
16. Una vez configurados los temas correctamente, solamente faltaria cargar el módulo y sus tablas. Ir a la carpeta en donde está el módulo del sistema interno `/public_html/dev/sites/all/modules/~module_name~` y copiarlo y pegarlo en la misma dirección pero del drupal de la página Web.
17. Ir a `admin/modules` y activar el módulo del sistema interno.
18. Hasta este punto, el módulo deberia de funcionar, pero como no tiene información, dará error. Entonces, hay que agregar todas las tablas del sistema interno, las tablas normalmente tienen como prefijo el nombre del modulo, entonces no deberia de ser dificil identificarlas. Una vez sepas cuales tablas son, solamente hay que crearlas en la base de datos de la página web, y meterle los datos del sistema interno.
19. Muy probablemente ahora sigan apareciendo cosas raras, es por los bloques de drupal. Hay que irse a `structure/block/list` y filtrarlos por los bloques del tema de interno, y quitar todos los que pertenecen a la página web.
20. Después de haber eliminado todos los bloques, ya no aparecen cosas raras, pero ahora solamente falta quitar los menus que pertenescan a la página Web, y copiar todos los menus que el sistema tenia aqui a este nuevo y asignarlos a los usuarios.
21. Probablemente existan problemas con los menús, por que en la página web solamente se este utilizando el main menu, y el sistema interno maneja varios menus. No deberia de haber problema para crear los menus necesarios para el sistema interno y asignarselos a los usuarios, el problema es que como se esta utilizando el main menu, ese menu siempre va a estar visible y pues tendrán 2 menús los que esten dentro del sistema. Para solucionar este error, hay que crear otro menu con los links de main-menu, una vez creado, deshabilitar todos los links de main-menu para que no muestre nada, y el nuevo menu creado, hay que mostrarlo solo por el rol de anonymous user, para que solamente ese menu lo vean los que no esten logueados en el sistema, y también, en caso de ser necesario, agregarlo a algún rol utilizado para administrar el sitio de la página web y no del sistema.
22. Revisar que el sistema interno no estuviera usando bloques, menus, módulos, roles, permisos extras, fields de usuario, carpetas de imagenes, etc. Si estuviera utilizando algunos de estos mencionados, se tendrían que agregar porque en ninguno de los pasos anteriores se hizó esto.
23. Todo deberia de estar funcionando correctamente :)
        
