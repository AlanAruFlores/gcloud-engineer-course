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



## Cloud Storage

- permite  almacenar y recuperar datos a nivel mundial y en cualquier momento
- podemos  usar cloud storage para entregar  contenidos de sitios web, almacenar archivos y distribuir los datos a traves de la descarga.
- se trata de una coleccion de buckets  en lo que se colocan objetos
- dispone de **tipos de clases**
	- *Standard:* es ideal para datos que se acceden con frecuencia o que se almacenan por periodos breves. Es la mas costosa pero no tiene duracion minima de almacenamiento ni costos de recuperacion
	- *Nearline:* es muy asequible y duradero  para datos  de acceso poco frecuente, como contenido multimedia de cola larga, copias de seguridad y archivado de datos. La minima duracion de acceso es de 30 dias
	- *Coldline:* es un servicio de muy bajo costo y muy duraradero  para almacenar datos de acceso poco frecuente. El acceso minimo es de 90 dias y tiene mayor costo de acceso a los datos
	- *Archive:* es el servicio mas duradero y de menor costo para el archivado  de datos, las copias de seguridad en linea y la recuperacion ante desastres. Es la mejor opcion para datos a los que accedes menos de una vez al año
- se dividen en varios elementos diferentes:
	- *buckets:* deben tener un nombre nivel unico global. Los datos son objetos que heredan su clase de almacenamiento y pueden ser archivos de texto, documentos, videos, etc.
	- *objetos:* son archivos de texto, documentos, videos, etc.
	- *Access:* para accederlo podemos usar **comando de gcloud** o **RESTFUL JSON API o XML API**

#### Features de Cloud Storage  
- El cliente puede proporcionar su propia clave de encriptacion en lugar de las que ofrece Google Cloud
- tambien proporciona la administracion del ciclo de vida de los objetos, lo que permite eliminar o archivar el objeto automaticamente
- control de versiones de objetos, esto permite mantener multiples versiones de objetos en el bucket.
- sincronizacion de directorios para que pueda sincronizar un directorio de VM con un bucket.
- las notificaciones de los objetos  se pueden configurar para Cloud Storage mediante Pub/Sub
- la AutoClass gestiona todos los aspectos de las clases de almacenamiento para un bucket
- podemos especificar **acciones** en  "la administracion del ciclo de vida" los objetos, si cumplen ciertas condiciones.
- la "funcion de bloqueo de retencion de objetos" le permite establecer la configuracion de retencion en objetos dentro de los buckets de Cloud Storage que tenga habilitada la funcion.

#### Data import services
![[Pasted image 20251110081844.png]]
- *Transfer Appliance:* es un dispositivo de hardware que puede utilizar para migrar  de forma segura grandes volumenes de datos (cientos de terabytes hasta 1 petabyte)
- *Storage Transfer Service:* permite importaciones de datos en linea de alto rendimiento. Esa fuiente puede ser de otro bucket de Cloud Storage , bucket S3 o una ubicacion HTTP/S
- *Offline media import:* es un servicio de terceros en lo que los medios fisicos se envian a un proveedor  que carga los datos



## FileStore
![[Pasted image 20251110222021.png]]
- Es un servicio de almacenamiento administrado para aplicaciones que requieren una interfaz de sistemas de archivos y un sistema de archivos compartido para datos.
- Podemos ajustar la capacidad y el rendimiento de FileStore en forma independiente genera un rendimiento previsibile más agil para los workloads basadas en archivos
- Admite cualquier cliente con NFSv3
- Obtiene beneficios como el "rendimiento escalable", cientos de terabytes de capacidad y el bloqueo de los archivos
- Con esto, podemos migrar las aplicaciones empresariales mas rapido
- FileStore admite una amplia variedad de aplicaciones empresariales que requieren de un sistema compartido
- EDA (Electronic Design Automation) se basa en la administracion de datos. Requiere la capacidad  de dividir los workloads en lotes en miles de nucleos.
- FileStore ofrece latencia baja para operaciones de archivos y, cuando cambian las necesidades de rendimiento o capacidad puedes aumentar/disminuir el tamaño de las instancias



## Cloud SQL
![[Pasted image 20251110222951.png]]
- es un servicio totalmente gestionado por base de datos MySQL, PostgresSQL o Microsoft SQL Server
- los parches  y las actualizaciones se aplican automaticamente pero aun asi hay que administrar los usuarios con las herramientas de autenticacion nativas que vienen  con estas bases de datos
- Cloud SQL admite muchos clientes
	- gcloud sql
	- App Engine, Google Workspace scripts
	- Application and tools:
		- SQL Workbench, Toad
		- External applications usando drivers de MySQL
- Ofrece alto rendimiento y escalabilidad
	- con hasta 64 TB de capacidad de almacenamiento
	- 60000 IOPS
	- 624 GB RAM por instancia
	- puede escalar facilmente hasta 96 nucleos de procesador y escalar horizontalmente con replicas de lectura

#### Cloud SQL Services
![[Pasted image 20251110224116.png]]
- *HA Configuration*: la configuracion se compone de una instancia primaria y una instancia de reserva. Mediante la replicacion sincrona, todas las escrituras hechas en la instancia principal se replican en los discos de ambas zonas. En caso de un fallo, la instancia *StandBy* se convierte en la principal.
- *Backup Service:* Cloud SQL proporciona copias de seguridad automatizadas
- *Import/Export:* puedes importar y exportar bases de datos usando **mysqldump**, o importar y exportar archivos CSV
- *Scaling:* Cloud SQL tambien puede escalar



![[Pasted image 20251110224808.png]]
- Si vamos a conectar nuestra Cloud SQL con nuestras instancias en el mismo proyecto, garantizamos que sea segura la conexion debido a que se acceden mediante IP privadas y no queda expuesta
- Si vamos a conectar con una aplicacion externa tenemos 3 opciones:
	- Usar *Cloud SQL Auth Proxy* que se encarga de la autenticacion, el cifrado y la rotacion de claves por usted (Recomendado)
	- Si necesitamos el control SSL, entonces podemos hacerlo manualmente: generando y rotando periodicamente los certificados
	- Podemos autorizar (sin cifrar) una IP especifica para conectarse a nuestra Cloud SQL}


## Arbol de desiciones
![[Pasted image 20251110224855.png]]



## Spanner
![[Pasted image 20251112224254.png]]
- Es un servicio diseñado para la nube, para combinar las ventajas de la estructura de la bases de datos relacionales  con la escalabilidad horizontal no relacional
- Proporciona petabytes de capacidad y ofrece consistencia transaccional a escala global, esquemas, SQL y replicacion sincronica automatica para alta disponibilidad
- Spanner ofrece lo mejor de los mundos relacionales y no relaciones.


![[Pasted image 20251112224835.png]]
- Una instancia Spanner replica datos en N Zonas de la nube, pueden estar dentro de una region o en varias regiones.
- La **ubicacion** de la base de datos es configurable
- Esta arquitectura permite una alta disponibilidad y una ubicacion global

## AlloyDB
![[Pasted image 20251112225344.png]]
- Es para PostgreSQL
- Es un servicio de base de datos totalmente administrado y compatible a PostgresSQL
- Esta diseñado para cargas de trabajo exigentes como el procesamiento hibrido transaccional y analitico
- Combina un motor de bases de datos desarrollado por Google con una **arquitectura multinodo** basada en la nube. Para ofrecer rendimiento, fiabilidad y disponibilidad 
- Permite copias de seguridad, replicacion, aplicacion de parches y gestion de capacidad
- Proporciona un procesamiento transaccional rapido, adecuado para wokloads exigentes

## Firestore
![[Pasted image 20251112225738.png]]
- Es una base de datos de documentos NoSQL, nativa de la nube, sin servidor, rapida y totalmente administrada
- simplifica el almacenamiento, la sincronizacion, y consultas de los datos
- Sus librerias dan live syncronization y soporte sin conexion
- Ofrece funciones de seguridad
- Admite transacciones ACID
- Permite la replicacion multiregional
- Firestore tiene 2 modos:
	- *Datastore mode:* 
		- Es compatible con aplicaciones DataStore 
		- Las queries son muy consistentes
		- No hay limite en grupo de entidades
	- *Native mode:*
		- Nueva capa de almacenamiento fuertemente consistente
		- Un modelo de datos de coleccion y documentos
		- Actualizaciones en tiempo real
		- Librerias para clientes web y mobile

## BigTable
- Es una base de datos NoSQL totalmente  gestionada con capacidad de petabyte y latencia muy baja
- BigTable es en realidad la misma base de datos que impulsa muchos de los servicios principales de Google
- Es una excelente opcion tanto para operaciones operativas como analiticas
- Se integra facilmente con herramientas populares como  Hadoop, Dataflow y Dataproc
- Es compatible con HBase
- BigTable almacena datos en tablas masivamente escalables, cada uno de las cuales es un mapa de clave/valor ordenado
- Las tablase de Bigtable son **dispérsas**, es decir, si la celda no contiene informacion entonces no ocupa espacio.

![[Pasted image 20251113230730.png]]
- Esta es una version simplificada de la arquitectura de BigTable
- Una tabla en Bigtable se divide en bloques de filas contiguas, llamadas tablets para ayudar a equilibrar los workloads de las consultas
- Los **tablets** se almacenan en **Colossus**, el sistema de archivos de Google, en formato SSTable.
- Un **SSTable** proporciona un mapa persitente, ordenado e inmutable de claves y valores.


## Memory Store
![[Pasted image 20251113231332.png]]
- MemoryStore para Redis proporciona un servicio de almacenamiento de datos en memoria totalmente administrado, construido sobre una infraestructura escalable, segura y de alta disponibilidad gestionada por Google 
- Las Apps pueden tener mejor rendimiento aprovechando el servicio de Redis, altamente escalable, disponible y seguro
- Tambien automatiza tareas complejas como habilitar la alta disponibilidad, la conmutacion por error, la aplicacion de parches y la monitorizacion
- Admite instancias hasta 300GB y un rendimiento de red de 12 Gbit/segundo