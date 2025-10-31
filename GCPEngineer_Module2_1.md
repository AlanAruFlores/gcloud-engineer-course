# GCP Engineer - Core Infraestructure (Module 2 - Part 1)

## Introduccion a la computacion a la nube
La computacion en la nube es una forma de usar la tecnologia de la informacion con estas cincos cualidades de igual importancia
![[Pasted image 20251027214859.png]]
1. Los clientes reciben recursos de procesamiento segun demanda y autoservicio. Desde una interfaz web, los usuarios obtienen la potencia de procesamiento, el almacenamiento y la red  que necesitan sin intervencion humana.
2. Los clientes acceden a los recursos por internet desde cualquier lugar que se conecten.
3. El proveedor de servicios en la nube  posee un gran grupo de recursos y los asigna a ese grupo de usuarios.
4. Los recursos son elasticos , es decir flexibles, al igual que los clientes.
5. Por ultimo, los clientes pagan SOLO por lo que usan.


### Historia
![[Pasted image 20251027215646.png]]
- Todo comienza desde la **colocacion** (primera ola), permitia a los usuarios alquilar un espacio fisico en lugar de invertir.
- Luego aparece la segunda ola **virtualizacion**, donde los centro de datos virtualizados se parecen a los centros de datos de colocacion de antes. Aca coinciden los componentes fisicos del procesamiento **(servidores, CPU, discos, etc)**, pero ahora son virtuales.
	Aca las empresas mantuvieron la infraestructura donde sigue siendo "un entorno controlado" y "configurado para el usuario".
- La empresa de Google vio que se veia muy limitada por la virtualizacion. Para ello paso a una arquitectura basada en contenedores, una nube completamente automatizada y elastica, con una combinacion de datos escalables y servicios automatizados


## IaaS & PaaS & SaaS


### IaaS
- IaaS (Infraestructura como servicio) brindan capacidades de computacion sin procesar, almacenamiento y redes organizadas en recursos similares a los centros de datos fisicos.
- **Compute Engine** es un ejemplo de servicio de IaaS de Google Cloud
- Los clientes por adelantado los recursos.
- 

### PaaS
- Vinculan el codigo a bibliotecas con acceso a la infraestructura que la app necesita. Esto permite que mas recursos se centren en la logica de la aplicacion
- **App Engine** es un ejemplo de servicio como PaaS de Google Cloud
- Los clientes pagan solo los recursos que usan.

### SaaS
- Proporciona toda la pila de aplicaciones y entrega de una app completa basada en la nube, para que los cientes accedan y usen.
- Estas aplicaciones NO ESTAN instaladas en tu computadora local, sino que se ejecutan en la nube como un servicio y los usuarios finales las consumen directamente desde internet como: Gmail, Drive, Documentos Google, etc.


![[Pasted image 20251027220615.png]]
A nivel general esto permite que las empresas de hoy en dia se puedan concentrar mas en sus objetivos comerciales en lugar de estar gestionando la infraestructura y gastando mucho dinero.


## Serverless Cloud Computing
- La tecnologia sin servidores es un paso mas en la evolucion de la nube. Esto permite que los desarrolladores se centren mas en el codigo en lugar de configurar el servidor.
- Las tecnologias sin servidor que incluye Google Cloud son:
	- **Cloud Run:** permite implementar aplicaciones basadas en microservicios alojados en contenedores en un entorno completamente administrado
	- **Cloud Run Functions:** usa codigo basado en eventos, como un servicio de pago.


## Google Cloud Network

![[Pasted image 20251027221449.png]]

- La red de Google esta hecha para brindar a los clientes el mayor rendimiento posible y las latencias mas bajas posibles para sus aplicaciones aprovechando mas de 100 nodos de almacenamiento  en cache de contenido en todo el mundo. Estos nodos permiten que las respuestas a los clientes sean muchas mas rapidas.
- Las ubicaciones de Google Cloud respaldan todo el trabajo importante que hacemos para nuestros clientes, desde regiones redundantes hasta alto ancho de banda a traves de cables submarinos.

![[Pasted image 20251027224318.png]]

- La infraestructura de Google esta basado en 7 ubicaciones principales: North America, South America, Europe, Middle East, Africa, Asia y Australia
- Tener multiples ubicaciones del servicio es importante porque nos permite poder elegir donde nuestras aplicaciones tendran mas disponibilidad, durabilidad y la latencia sera mejor en esa region.
- La **latencia** mide el tiempo que tarda un paquete de informacion en viajar desde su origen hacia su destino

![[Pasted image 20251027223703.png]]
- Cada una de estas ubicaciones se dividen en VARIAS regiones y zonas diferentes.
- Las **regiones** representan area geograficas independientes y estan compuestas por zonas. Ejemplo **Londres** o europe-west2 es una region que se compone de **3 zonas**

![[Pasted image 20251027223857.png]]
- Una **zona** es un area donde se implementan los recursos de Google Cloud. Ejemplo si iniciamos un VM en Compute Engine, se ejecutara en la zona que especifique para garantizar la redundancia de recursos.


![[Pasted image 20251027224246.png]]
Algunos recursos de Google permiten colocar recursos a varias regiones, denominada **multiregion**. Por ejemplo, **Spanner** permite replicar los datos de las bases de datos NO SOLO en multiples en zonas, sino en multiples zonas en multiples regiones. Estas replicaciones adicionales permiten leer a baja latencia desde multiples ubicaciones  cercanas o dentros de las regiones.


Para encontrar los numeros mas actualizados de google acerca de las regiones y zonas es en: ***cloud.google.com/about/locations.***


## Google Security Infrastructure

![[Pasted image 20251027225241.png]]

La infraestructura de seguridad se explica con capas progresivas, que comienzan desde la seguridad fisica de los centros de datos, como se protegen el HW y SW que sustentan la infraestructura y las restricciones tecnicas y los procesos para respaldar la seguridad operacional.
- **Capa de Hardware**: Google diseño tantos las placas como los equipos de redes de sus centros de datos, chips personalizados para la seguridad que actualmente estan implementados en servidores y perifericos. Tambien usan una stack que garantiza que inicie correctamente el SW como **firmas criptograficas** por el BIOS, bootloader, kernel y la imagen del sistema operativo base. Por ultimo, incorpora la proteccion de seguridad fisica (medidas de seguridad fisica).
- **Capa de implementacion**: la caracteristica clave es la encriptacion de la comunicacion de los servicios.
	- Nuestra infraestructura ofrece integridad y privacidad criptografica a los datos de la llamada de procedimiento remoto (RPC) en la red.
	- Los servicios de Google se comunican entre si mediante el RPC. 
	- Se encripta automaticamente el trafico del RPC de la infraestructura que pasa entre los centros de datos.
- **Capa de identidad de los usuarios**: suele manifestarse a los usuarios finales como la pagina de acceso, va MAS ALLA de solo un usuario y contraseña. Se puede usar 2FA.
- **Capa de servicios de almacenamiento:** esta la encriptacion en reposo, la mayoria de las apps de Google acceden al almacenamiento fisico, o de archivos a traves de servicios de almacenamiento y la encriptacion de claves administradas. Ademas, Google tambien permite la encriptacion de hardware de **discos duros y SSD**
- **Capa de comunicacion en internet:** Los servicios de google estan disponibles a internet mediante una infraestructura llamada **Google Front End**. Esta asegura todas las conexiones TLS terminen con un par de claves privada/publica y un certificado x509. Ademas, GFE aplica proteccion contra ataques DoS (Denial of Service)
- **Capa de seguridad operativa**: cuenta con 4 claves
	- La primera es la deteccion de intrusiones: las reglas y la IA envian advertencias a los equipos de seguridad operativa de Google.
	- reduccion de riesgo de infiltracion: Google limita y supervisa intensamente las actividadesde los empleados con acceso de administrador a la infraestructura.
	- los empleados usan U2F (Universal Second Factor), para evitar ataque de phishing los empleados deben usar cuentas que usen llaves de seguridad compatible con U2F.
	- practica estricta de desarrollo de software: Google usa un control  de codigo fuente y una revision de la misma. Ademas brinda bibliotecas a sus developers que impiden que ingresen a ciertas clases  de errores de seguridad.

## Jerarquia de recursos de Google Cloud

![[Pasted image 20251028220752.png]]
La jerarquia de recursos de Google consta de 4 niveles
- Resources: representan los recursos como VMs, buckets de Cloud Storage, tablas BigQuery, etc.
- Projects: Los recursos se organizan en proyectos
- Folders: Los proyectos se pueden organizar en carpeta o subcarpetas
- Organization Node: abarca los proyectos, carpetas y los recursos de tu organizacion.

Tambien es importante tener encuenta la jerarquia de los recursos ya que:
- Dependiendo del nivel que apliquemos los permisos, esta va a impactar hacia un area especifica. Es decir, si agregamos permisos a nivel de Carpeta (**Folder**) se va a aplicar a todos los proyectos.
- Los **proyectos** son la base  para habilitar y usar servicios de Google Cloud: habilitar APIs, habilitar facturacion, agregar o eliminar colaboradores y añadir otros servicios de Google
- Un **proyecto** contiene los siguientes 3 atributos: Project ID, Project name y  Project number.
		- El Project ID es un identificador unico a nivel global. Google lo asigna y no puede cambiarse.
		- El Project Name son creados por el usuario, no tienen que ser unicos y pueden modificarse
		- El Project Number son numeros creados por Google que se usa para cosas internas.


### Resource Manager Tool

![[Pasted image 20251028221918.png]]
- Es una API que puede reunir una lista de los proyectos asociados a una cuenta, crear proyectos, actualizar, eliminar proyectos, etc.



## IAM


- Los administradores para restringir el acceso a ciertos recursos pueden usar **IAM (Identity Access Managment)**
- Con esto podemos aplicar politicas que definan "quien puede hacer que" y "en que recurso".
- El "quien" puede ser: usuario, grupo de usuario, cuenta de servicio o un dominio de cloud identity. Tambien puede ser el  **principal** que tiene su propio identificador que suele ser un correo electronico
		- ![[Pasted image 20251028222640.png]]

- el "puede hacer que" representa un **rol**. Un rol en IAM es una coleccion de permisos
- Se pueden aplicar tanto como permisos como permisos de denegacion. 

![[Pasted image 20251028222930.png]]
- Existen 3 tipos de roles.
- Basic: tienen permisos bastantes amplios. Estos roles son "Owner, Editor, Viewer y Billing Admin"
- Predefined: Los servicios de Google Cloud ofrecen conjuntos de roles predefinidos y definen hasta cuando pueden aplicarse. Ejemplo: En **Compute Engine** puedes aplicar roles predefinidos especificos, como InstanceAdmin.
	![[Pasted image 20251028223523.png]]
- Custom: se usa cuando necesitamos un rol aun mas especifico. Muchas empresas usan un modelo denominado "privilegio minimo", donde cada persona posee la minima cantidad de permisos necesarios para hacer su trabajo. 


## Cuenta de Servicio
Imagina que necesitamos que nuestra VM necesita acceder con frecuencia a otros servicios en la nube. Para ello en lugar de dar cada rato dando acceso a la VM a dicho servicio, podemos darle permisos directamente a la VM.
Las cuentas de servicio permiten dar permisos a una maquina virtual para que interactue con otros servicios SIN INTERVENCION HUMANA

![[Pasted image 20251028224013.png]]

- Imagina que necesitamos hacer que ningun trafico que venga de internet pueda acceder a los datos del Cloud Storage, solamente queremos que una VM pueda tener acceso a la misma. Para ello podemos hacer uso de las cuentas de servicio.



## Cloud Identity
![[Pasted image 20251028224719.png]]
- Permite definir politicas y administrar los usuarios y grupos desde la consola del administrador de Google.
- Los administradores acceden a los recursos de Cloud con los mismos nombres de usuario y contraseñas que usaban en los sistemas de Active Directory o LDAP existentes.
- Cuando alguien deja la organizacion, el administrador puede eliminarlo del grupo.


## Interaccion con Google Cloud
![[Pasted image 20251028224830.png]]
###  Google Cloud Console

- Es la **interfaz web** (en el navegador) para **administrar los servicios** de Google Cloud.
- Permite crear máquinas virtuales, bases de datos, configurar redes, ver costos, etc.  
    👉 Es como el “panel de control” visual de tu cuenta de Google Cloud.

### Google Cloud SDK (Software Development Kit)

- Es un **conjunto de herramientas de línea de comandos** que se instalan en tu computadora.
    
- Incluye comandos como `gcloud`, `gsutil`, `bq`, etc.  
    👉 Sirve para **gestionar recursos de Google Cloud desde la terminal**, sin usar el navegador.
    

---

### Cloud Shell

- Es una **terminal basada en la nube** que viene preconfigurada con el Google Cloud SDK y otras herramientas.
    
- Se ejecuta directamente desde la **Google Cloud Console** (sin instalar nada en tu PC).  
    👉 Ideal para ejecutar comandos de `gcloud` fácilmente.

###  APIs (Application Programming Interfaces)

- Son **puntos de acceso programables** que permiten que tus aplicaciones se comuniquen con los servicios de Google Cloud.  
    👉 Por ejemplo, puedes usar la **API de Vision** para analizar imágenes o la **API de Maps** para geolocalización.
    
### Google Cloud App

- Generalmente se refiere a **App Engine**, un servicio de Google Cloud que permite **desplegar aplicaciones web** sin preocuparte por la infraestructura.  
    👉 Solo subes tu código, y Google maneja el escalado, servidores y mantenimiento automáticamente.


## Redes Virtuales en Compute Engine 

![[Pasted image 20251029223728.png]]
- Una nube privada virtual o VPC es un modelo privado, individual y seguro de computacion de la nube alojado dentro de una nube publica como Google Cloud.
- En una VPC los usuarios ejecutan codigo, almacenan datos, alojan sitios webs, etc.
- La VPC se aloja remotamente en un proveedor de servicios
- Las VPC combinan la escalabilidad y conveniencia de la computacion de nube publica.
- Las VPC conectan los recursos de Google Cloud entre si y a internet.
- Las VPC son redes globales y pueden tener consigo otras subredes
- Las subredes abarcan las zonas que forman una region
- Se puede expandir una subred ampliando mas los rangos de direcciones IP.


## Compute Engine
![[Pasted image 20251029224006.png]]
- Con compute engine, los usuarios pueden crear y ejecutar VMs en la infraestructura.
- No hace falta realizar inversiones iniciales
- Se pueden ejecutar miles de CPU virtuales en un sistema diseñado para ser rapido y ofrecer un rendimiento constante.
- Cada VM tiene la potencia y funcionalidad de un SO
- Cada VM se puede configurar como un servidor fisico.


![[Pasted image 20251029224319.png]]
- Las VMs se pueden crear con la herramienta consola de Google Cloud basada en la Web para administrar proyectos y recursos de Google Cloud, la API de Compute Engine o Google Cloud CLI.
- La instancia puede ejecutar imagenes de Linux y Windows Server o cualquier imagen.
- Se pueden crear imagenes y ejecutarlos.
- Las VMs en Compute Engine se facturan por segundo
- Compute Engine ofrece descuentos por compromiso de uso


