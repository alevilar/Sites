# Tareas en Proceso

no hay tareas puntuales para este plugin

# Pendientes

no hay pendientes registrados


 
  
# Tareas Realizadas


- hacer que se cargue el Rol del usuario en la variable de sesion "MtSites.current_role" con el valor Rol.machine_name (esta tabla es levantada solo en los tenants)


- arreglar rutas
  - por un lado hay que hacer que todas sean hechas con array (forma cake) y no un string.
  - por otro lado hay que hacer que el :tenant sea parmanente durante la navegacion



-- Hacer que el Plugin MultiTenant pueda detectar un tenant usando los parametros de la url, mediante Routes.  
Asi como Cake detecta un plugin, controller y action, seria buenisimo si existiese la forma de agregarle el sitio. Por ejemplo, encontré que algo "similar" a esto es cuando se usa el "lang"
por ejemplo, para http://example.com/en/users/

 {{{
 * Router::connect(
 *   '/:lang/:controller/:action/:id',
 *   array(),
 *   array('id' => '[0-9]+', 'lang' => '[a-z]{3}')
 * );
 }}}


- Investigacion: como hacer que los subdominios sean mas dinamicos en apache (usar como wildcards). Se logro pero hay que tocar el archivo hosts

Los siguientes 4 pendientes tienen su descripción detallada al final de este backlog, bajo el titulo de **"Anexo I"**
- Usar Sessions para guardar datos del sitio actual.
- Crear ModelSites
- Crear MtSitesBehavior
- Crear MtSitesAuthorize
 

- Refactorizar codigo para usar carpeta con nombre del sitio para archivos del webroot
- Seguridad para que otros usuarios no pueda acceder a la carpeta de imagenes de otros sitios
- Incluir Plugin Upload.Upload para administrar archivos como attachments
- script de instalacion:
  - crear subdominio en apache. Ver si se puede hacer con un alias.
  - establecer permisos ACL del primer usuario
  - Crear carpeta de usuario en webroot/files
  - crear BD para tenat
  - notificar por mail
  - actualizar tabla principal con usuario y sitio

- agregar Panel administrativo en sitio
- automatizar instalacion nuevo sitio


- tareas de Investigacion
  - cargar tenant con su base de datos propia
  - usar subdominios como tenants


# Anexo I

##### Sessions
Usar **Sessions** para almacenar datos del sitio actual. Este punto es algo similar a lo que hace el Auth component con los datos del usuario que pueden ser accedidos como `$this->Session->read('Auth.User.name');`
En este caso, es necesario poder acceder a algo similar a esto:  
```
  $this->Session->read('Auth.User.Site.name');
  o $this->Session->read('Auth.Site.name');
```

##### Model Sites
Crear **Model Sites** con los siguientes campos:
- id
- name
- user_id
- databaseHost
- databaseUser
- databasePass
-  ...  todos los campos requeridos por DATABASE_CONFIG de Cake
  
Notar que 1 Sitio pertenece a 1 usuario


##### MtSitesBehavior
Crear **MtSitesBehavior**. Alli aplicaremos toda la lógica Multi Tenant que ayudará a decidir, para cada modelo, a que base de datos apuntar seteando el atributo del modelo *$useDbConfig*. 
Ej: public `$useDbConfig = site_my_nice_paxapos`;
Cuando el Behavior inicia, se crea, "on the fly", el array de configuracion para el sitio actual utilizando el    metodo  ConnectionManagger::create($name, $configArray)

Para mayor información sobre "ConnectionManagger create" method. 
[Ver Documentación Cake] (http://api.cakephp.org/2.5/class-ConnectionManager.html#_create)

##### MtSitesAuthorize
Crear nuevo **MtSitesAuthorize** donde se manejará la autorización para acceder a los sitios según al sitio que pertenezcan.
[Ver Documentación Cake](http://book.cakephp.org/2.0/en/core-libraries/components/authentication.html#creating-custom-authorize-objects)
	
	
