# Essential Google Cloud Core Services


## IAM (Identity and Access Managment)

![[Pasted image 20251109135053.png]]
- Es una forma de identificar "quien puede hacer que con que recurso"
	-  "quien" puede representar una persona, un grupo o aplicacion 
	- "el que" se refiere a los permisos o acciones
	- "recurso" puede ser cualquier servicio de google cloud 
- IAM se compone de diferentes objetos como se muestra  en esta diapositiva


## IAM resource hierarchy
![[Pasted image 20251109135517.png]]
- Los recursos de Google Cloud estan organizados jerarquicamente, como se muestra en la estructura de arbol-
- El **nodo de organizacion** es el nodo raiz de la jerarquia
- Las **carpetas** son los hijos de la organizacion
- **Los projects** son los hijos de las carpetas
- **Los recursos** individuales son hijos de los proyectos
- Los roles IAM declarados en el nodo de organizacion se heredan tambien para todos los recursos.
- Los roles IAM declados en los folder se heradan a los recursos que SOLO esta contiene

#### Organization node
![[Pasted image 20251109140348.png]]
- Es el nodo raiz de la jerarquia de recursos de Google Cloud
- Este nodo tiene muchas funciones, como la de admininstrador de la organizacion. Este rol se debe usar con mucho cuidado porque todos los recursos dependen de ella.
- El *administrador de la organizacion* proporciona a un usuario como Bob acceso para administrar todos los recursos que pertenecen a su organizacion lo cual es util para la auditoria
- Tambien existe el rol *Creador de projects* que permite un usuario como Alice crear proyectos dentro de su organizacion
- Tambien existe un rol superior al de organization node, el cual es *Google Worksspace/Cloud Identity super administrator*, sus responsabilidades son:
	- asginar el rol de "organization admin" a los usuarios
	- ser un punto de contacto en caso de problemas de recuperacion
	- controlar el ciclo de vida de la cuenta de Google Workspace/Cloud Identity y de los recursos
- Las responsabilidades de  *Organization admin* son:
	- definir las politicas de IAM
	- determinar la jerarquia de los recursos
	- delegar las responsabilidades sobre componentes criticos como redes, facturacion y jerarquia de recursos, a traves de roles IAM


#### Folders
![[Pasted image 20251109141412.png]]
- pueden considerarse como suborganizaciones dentro de una organizacion
- los folders proporcionan un mecanismode agrupacion adicional y un limite de aislamiento entre proyectos
- los folders pueden usarse para modelar diferentes  entidades legales, departamentos y equipos de una empresa
- cada folder puede tener otras subfolders


A continuacion se muestra los roles de esta jerarquia:_
![[Pasted image 20251109141518.png]]




## IAM Roles
![[Pasted image 20251109154410.png]]
- Existen 3 tipos de roles:
	- *Basic*: son los roles originales que estaban disponibles en la consola de Google Cloud, pero son amplios. Aplican a un proyecto de Google Cloud y afectan a todos los recursos de ese proyecto. Uno de ellos son: Owner, Editor, Viewer, Billing Administrator.
	- *Predefined:* estas definen donde se aplican estos roles. Esto permite a los miembros acceder a recursos **especificos** de Google Cloud. Estos se componen de muchos permisos.
		- ![[Pasted image 20251109154906.png]]
		- Aca por ejemplo se indica que en el project_a los miembros de un grupo tendran como rol "Instance Admin Role" el cual consigo tiene la siguiente lista de permisos sobre las VMs
		- Tiene 3 roles predefinidos principales: Compute Admin, Network Admin y Storage Admin
	- *Custom:* son roles muchos mas especificos, donde nos va permitir trabajar algo denominado "privilegios minimo" donde vamos a setear SOLO los permisos necesarios.

## Members
![[Pasted image 20251109155852.png]]
- Existen 5 tipos de miembros
	- Google Account: representa una persona (admin, desarrollador, etc) que interactue con Google Cloud. Cualquier que tenga el dominio "gmail.com" es considerado como una cuenta de Google.
	- Account Service: representa  a la cuenta de una aplicacion en lugar de un usuario final. Cuando ejecutas codigo alojado en Google Cloud, especificas la cuenta con la que debe ejecutarse el codigo.
	- Google Group: Es una coleccion de varias cuentas de Google y cuentas de Servicio. Tiene su dominio especifico del grupo.
	- Google Workspace Domain: representa un grupo virtual de todas las cuentas de Google que se han creado en la cuenta de Google Workspace de una organizacion
	- Cloud Identity Domain: Los clientes de Google que no son clientes de Google WorkSpace pueden obtener las mismas capacidades a traves de Cloud Identity

### IAM Allow Policies
![[Pasted image 20251109160918.png]]
- La politica de permiso controla el acceso al recurso y a cualquier descendiente de ese recurso que herede dicha poltiica
- Una politica asocia a una o mas **principales (*conocidos como miembros o identidad*)**

### IAM Deny Policies
- las politicas de denegacion de IAM permiten establecer limites  de acceso a los recursos de Google Cloud
- Podemos definir reglas de denegacion que impidan ciertas entidades utilicen cierto permisos
- Estas politicas se componen de un conjunto de reglas de denegacion
- Opcionalmente podemos definir una condicion que debe cumplirse para que se deniegue el permiso

### IAM Conditions
- permiten definir y aplicar un control de acceso condicional basado  en atributos para los recursos de Google Cloud
- con esto podemos otorgar acceso a las entidades (miembros) a ciertos recursos si cumplen una condicion


## Service Account
- Como comentamos antes, es una cuenta perteneciente a una aplicacion en lugar de un usuario
- Podemos  habilitar las cuentas de servicio y otorgar acceso de  lectura/escritura a la cuenta en la instancia donde planea ejecutar su aplicacion
- Este tipo de cuentas se identifican con una direccion de correo 
- *Default compute engine service account*
	- Este tipo de cuenta son las que estan por defecto y se identifican con "numero-del-pryuecto@developer.gserviceaccount.com"
	- Se le otorga automaticamente el rol de Editor en el proyecto
	- Por defecto, la cuenta de servicio se habilita para esa instancia
- Los roles de las cuentas de servicio pueden asignar a grupos o usuarios


![[Pasted image 20251109163342.png]] 
- Se pueden definir scopes (ambitos) donde dependiendo del ambito, la cuenta de servicio de aplicacion aplicara ciertos permisos