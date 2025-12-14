
# Introduction to Google Cloud

## Introduction
- La computacion en la nube es una forma de usar la tecnologia de la informacion (IT), tiene estas caracterisiticas importantes:
	- En primer lugar los clientes obtienen los recursos informaticos bajo demanda y de autoservicio. A traves de una interfaz web, los usuarios obtienen la potencia de procesamiento, el almacenamiento y la red que necesitan
	- En segundo lugar, los clientes obtienen acceso a esos recursos a traves de internet, desde cualquier lugar donde tengan conexion.
	- En tercer lugar, el proveedor de la nube tiene un gran grupo de recursos y los asigna a los usuarios
	- En cuarto lugar, los recursos son elasticos, significa  que son flexibles, por lo que los clientes pueden
	- Por ultimo, los clientes pagan solo por lo que usan o reservan a medida que avanzan


## Google Cloud compute offerings
Google  ofrece un a variedad  de servicios informaticos, exploremos cada uno:

#### Compute Engine
- Compute Engine es una oferta de IaaS, o infraestructura como servicio, que proporciona computacion, almacenamiento y red de manera virtual similar a los centros de datos fisicos
- proporciona acceso a configuraciones de maquinas virtuales predefinidas y personalizadas
- Las VMs necesitan almacenamiento en bloque y Compute Engine ofrece 2 opciones principales
	- discos persistentes: ofrecen almacenamiento en red que puede escalar hasta 257 TB y puede almacenar snapshots de disco para realizar copias de seguridad y movilidad
	- ssd locales: permiten realizar numerosas operaciones de entrada y salida por segundo
- Los workloads de Compute Engine se pueden colocar detras de balanceadores de carga globales que admiten escalamiento automatico
- Los costos se pueden administrar por la ayuda de facturacion por segundo
#### Google Kubernetes Engine
- GKE ejecuta aplicaciones en contenedores en un entorno de la nube
- Un contenedor representa el codigo empaquetado con todas sus dependencias
#### App Engine
- App Engine es una oferta PaaS totalmente administrada, o plataforma como servicio.
- Las ofertas PaaS vinculan el codigo a las bibliotecas que brindan acceso a las necesidades de la aplicacion de la infraestructura
- Esto permite concentrarse mas recursos en la logica de la aplicacion en lugar de la implementacion
- Esto significa que solo se necesita cargar el codigo y App Engine implementara la infraestructura requerida
- Esta integrado con otros servicios como Cloud Monitoring, Cloud Logging, Cloud Profiler and Error Reporting
#### Cloud Run
- Es una plataforma administrada que ejecuta contenedores sin estado a traves de solicitudes web o eventos  Pub/Sub
- Es serverless, significa que  elimina las tareas de gestion de la infraestructura para solo centrarse en el codigo
- contruido sobre Knative, una API abierta y un entorno de ejecucion creado en Kubernetes que permite mover su workloads entre diferentes entornos y plataformas.
- Es rapido
- Cobra solo por los recursos usados, calculados hasta los 100 milisegundos mas cercanos
#### Cloud Run Functions
- es una solucion informatica asincrona, liviana y basada en eventos para crear funciones pequeñas y de proposito unico que responden a eventos de la nube sin la necesidad de administrar un servidor  o un entorno de ejecucion
- Ejecuta codigo en respuesta a eventos, ejemplo si se carga algo en cloud storage se dispara el evento
- Las funciones cloud run a menudo se denominan funciones como servicio


## Google Cloud Network
- Detras de los servicios que ofrece Google Cloud, se esconde una variedad de recursos, desde activos fisicos  como servidores hastas recursos virtuales como VMs y contenedores
- Todo funciona en la Red Global de Google
- La red de Google transporta hasta el 40% del trafico de internet del mundo cada dia- La red de Google es la red mas grande de su tipo y Google ha invertido miles de millones de dolares a lo largo  de los años para construirla
	- Dispone de bajas latencias
	- 100 nodos distribuidos en el mundo para el caching de los contenidos
	- La alta demanda de los recursos es cacheada para un rapido acceso
	

![[Pasted image 20251213121010.png]]
- Esta compuesta de 7 partes geograficas:
	- North America
	- South America
	- Africa
	- Middle East
	- Europe
	- Asia
	- Australia
- Tener ciertos lugares la aplicacion, es importante  porque elegir  donde ubicar la ubicacion afecta: Disponibilidad, Durabilidad, Latencia


## Resource Managment
![[Pasted image 20251213121546.png]]
- Cada recurso de Google que uses debe pertenecer a un proyecto
- Un **proyecto** es un contenedor para todos tus recursos de Google Cloud
	- permite organizar  recursos, administrar la facturacion y controlar el acceso
	- tiene un identificador unico
- Se compone de una jerarquia de recursos, las cuales de manera ascendente es:
	- Recursos
	- Proyectos
	- Carpetas
	- Nodo de organizacion
- Es importante comprender esta jerarquia de recursos porque se relaciona con la forma en que se administrar y aplican las politicas  cuando se usa Google Cloud
- Las politicas se pueden definir a nivel de Proyecto, Carpeta y Nodo de organizacion
- Las politicas se heredan de padre a hijo, es decir, si agregar alguna politica a nivel de Folder, entonces esta se hereda al proyecto

#### IAM 
![[Pasted image 20251213122235.png]]
- Con IAM, podemos definir politicas que digan "quien puede hacer que" y "en que recurso".
- El "quien" de una politica de IAM puede ser: una cuenta o un Grupo de Google, una cuenta de servicio o un dominio de Cloud Identity


## Billing
![[Pasted image 20251213122933.png]]
- La facturacion se define a nivel de proyecto en la jerarquia de recursos de Gogle Cloud. Por eso cuando definimos un proyecto debemos vincular una cuenta de facturacion
- Las cuentas de facturacion se cargan automaticamente y se facturan cada mes  o en cada limite umbral
- Puede usar cuentas secundarias para separar la facturacion por proyecto.
- Podemos definir herramientas de facturacion:_
	- **Budgets:** Podemos definir un presupuesto limite fijo
	- **Alerts:** crea alertas  para recibir una notificacion cuando se alcance el limite del presupuesto
	- **Reports:** es una herramienta visual para supervisar el gasto en funcion de un proyecto o servicio
	- **Quotas:** se diseñaron para evitar el consumo excesivo de recursos  debido a errores o ataques maliciosos

# Introduction to Containers and Kubernetes

## Container
- La virtualizacion permitio ejecutar varios servidores virtuales y sistemas operativos en una computadora local
- La capa de SW que borra las dependencias de un SO del hardware y permite  que varias  maquinas virtuales compartan el HW, se llama **hipervisor**
- La VM basada en kernel o KVM es un **hipervisor**
- Hoy con la virtualizacion podemos  implementar servidores y soluciones nuevas, se desperdician menos recursos y la portabilidad es mejor ya que se pueden crear *imagenes* de las VMS para moverlas


![[Pasted image 20251213141432.png]]
- Si tenemos 2 aplicaicones en una misma VM que comparten la misma dependencia puede traer errores, porque si se actualiza una dependencia la otra puede ser que deje de funcionar
- Podria solucionarse capaz aplicando ingenieria de SW bloqueando la actualizacion de la dependencia para que ninguna app pueda hacer cambios
- Pruebas de integracion para verificar que siga andando correctamente el flujo



![[Pasted image 20251213141608.png]]
- Para la solucion anterior, podemos dedicar cada aplicacion en cada VM 
- Aca cada app mantiene aislado sus dependencias de la otra. Aca tambien se aisla el kernel para que el rendimiento de una no afecte a la otra
- Pero la desventaja de esto es que si escala cientos de miles de instancias, se observaran limitaciones


- Entonces aca entran los containers, son espcaios de usuario aislados en la que se ejecuta el codigo de la aplicacion
- Son ligeros ya que no disponen de un SO completo, se pueden programar o integrar en el sistema subyacente y se pueden prender / apagar rapidamente.
- No precisan iniciar una VM o el SO de cada aplicaicon
- El container puede ejecutar codigo final en las VMs, El codigo de las apps se empaqueta con las dependencias  necesarias y el motor que ejecuta el contenedor las pone a disposicion en el entorno de ejecucion
## Container Images
- Una aplicacion y sus dependencias se denominan "imagen"
- Un contenedor es solo una instancia en ejecucion de una imagen
- Para crear y ejecutar imagenes en contenedores, necesitamos usar Docker
- Un contenedor tiene el poder de aislar las cargas de trabajo, para ello proviene de la siguiente combinacion:
	- **Linux Proccess**: cada proceso de linux  tiene su propio espacio de direcciones  de memoria virtual, los procesos de linux pueden crearse/destruirse rapidamente
	- **Linux namespaces**: los contenedores usan espacios de nombres de linux para controlar lo que una aplicacion puede ver, como "numero de identificacion de procesos", "arboles de directorios", "direcciones IP", etc.
	- **Linux cgroups**: controlan lo que una aplicacion puede usar, como su consumo maximo de tiempo CPU, memoria, ancho de banda de E/S y otros recursos
	- **Union File System**: para agrupar todo lo necesario en un paquete ordenado

![[Pasted image 20251213143047.png]]
- una imagen de contenedor esta estructurada en capas y la herramienta usada para construir la imagen lee instrucciones de un archivo llamado **manifiesto del contenedor**
- para las imagenes de contenedor con formato Docker, se denomina DockerFile
- cada instruccion en el Dockerfile especifica una capa dentro de la imagen  del contenedor.

![[Pasted image 20251213144619.png]]
- un Dockerfile contien 4 comandos cada un de las cuales crea una capa
	- la declaracion *FROM* comienza creando una capa base , que se extrae  de un repositorio publico. Resulta que es el entorno de ejecucion, Ej: Linux
	- el *COPY* contiene algunos archivos copiados desde el directorio actual de su herramienta de compilacion
	- el *RUN* compila la aplicacion usando el comando "make" y coloca los resultados de la compilacion en una tercera capa
	- el *CMD* especifica que comando debe ejecutarse  dentro del contenedor  cuando se lanza

**Nota: Google mantiene  Artifact Registry en pkg.dev que contiene imagenes publicas y de codigo abierto. Tambien hay otras plataformas como Docker Registry, Git Lab, etc.**


#### Google Clone Build
![[Pasted image 20251213145225.png]]
- Google ofrece un servicio administrado para crear contenedores  llamado Cloud Build
- Cloud build esta integrado con Cloud IAM y fue diseñado para recuperar compilaciones  de codigo fuente de diferentes  repositorios de codigo, incluidos "Cloud Source Repositories"



## Kubernetes
![[Pasted image 20251213150822.png]]
- Es una plataforma para administra cargas de trabajo y servicios en contenedores. Permite organizarlos en muchos hosts, escalarlos como microservicios e implementar rollouts y rollbacks
- Es un conjunto de APIS para deployar contenedores  en un Set de nodos, denominado Cluster
- Aqui un nodo es una instancia de procesamiento, como una maquina.
- Aca podemos definir como interactuaran las aplicaicones entre si y Kubernetes los llevara a cabo.
- Kubernetes permite la configuracion imperativa, en la que se emite comandos para cambiar el estado del sistema.

## Google Kubernetes Engine
![[Pasted image 20251213150845.png]]
- GKE es un servicio de Kubernetes administrado y alojado en la infraestructura de Google. Esta diseñado para ayudar a implementar, administrar y escalar entornos de Kubernetes para aplicaciones en contenedores 
- GKE esta completamente administrado, no es necesario aprovisionar los recursos subyacentes.
- GKE ofrece un modo *GKE Autopilot*, que esta diseñado para administrar la configuracion de su cluster como "nodos, escalamiento , seguridad y otras"
- cuando empezamos con GKE, genera un sistema Kubernetes que se denomina **Cluster**
- Las VMs que se alojan en contenedores en un Cluster GKE se denominan **nodos**
- permite  el escalado de las cargas de trabajo
- se integra a varios servicios como "Cloud Build" que usa contenedores que usan imagenes de forma segura de "Artifact Registry"
- se integra con Google Cloud Observability

# Kubernetes Architecture

## Kubernetes Concepts
![[Pasted image 20251213154151.png]]
- **Kubernetes Object Model**: un objeto representa cada elemento que administra Kubernetes. Puedes ver y cambiar los atributos y el estado de los objetos
- **Declarative Managment**: debes indicarle a kubernetes como administrar los objetos y trabajara para lograr y mantener el estado deseado
- Un objeto de kubernetes se define como una  entidad persistente que representa el estado "deseado" y el "actual" de un elemento que se ejecuta en un cluster.
- Varios tipos de objetos representan: Apps en contenedores, recursos disponibles y las politicas que influyen en su comportamiento


![[Pasted image 20251213154446.png]]
- Los objetos kubernetes tiene 2 elementos disponibles
	- El primero es la especificaicon para cada objetro que se creara. Aca definimos el estado deseado del objeto
	- El segundo es el estado actual del objeto que proporciona
- "control plane" hace referencia a herramientas para que funcione bien los clusteres de kubernetes
![[Pasted image 20251213162522.png]]
- cada objeto representa un "tipo" especifico
	- Los *pods* son los componentes basicos del modelo estandar de Kubernetes y son el objeto implementable mas pequeño de Kubernetes
	- Cada contenedor que se ejecuta en un sistema Kubernetes esta en un Pód
- El *Pod* crea el entorno en la que se encuentran los contenedores. Ese entorno puede alojar uno o mas contenedores 
- Kubernetes asigna una direccion IP unica a cada Pod y los contenedores del Pod comparten el espacio de nombres de la red, incluidos IP Address y los puertos de red.
- Un Pod puede especificar volumenes de almacenamiento para compartirlos entre sus contenedores

## Kubernetes components
![[Pasted image 20251213162754.png]]
- En primer lugar, un cluster necesita computadoras y estas computadoras generalmente son maquinas virtuales.
- Un computadora sera el "control plane" y los demas nodos
- El trabajo del nodo es ejecutar los **pods** y del "control plane" es coordinar todo el cluster
#### Componentes del Control plane
![[Pasted image 20251213164640.png]]
- **kube-APIServer**: es el unico componente con el que interactuaremos directamente. El fin de este es aceptar comandos que visualizan o cambian el estado del cluster, incluye lanzamiento de pods
- **kubtectl**: su fin es conectarse al kube-APIServer y comunicarse con el mediante la API de kubernetes
- **etcd**: es la base de datos del cluster. Su trabajo es guardar de manera confiable el estado del cluster. kube APIServer es el unico que interactuara con la base de datos.
- **kube-scheduler**: es responsable de programar  pods en los nodos. Evalua los requisitos de cada pod individual y selecciona que nodo es el mas adecuado. Si hay un pod sin un nodo entonces este se encarga de registrar en que nodo va a residir dicho pod, esto lo hace porque conoce los estados de todos los nodos
- **kube controller manager**: monitorea el estado de un cluster a traves de kube APIServer. Siempre que el estado del cluster no coincida con el deseado, este intentara llegar a hacer cambios para llegar a ese estado deseado.
- **kube-cloud-manager**: administra los controladores que interactuan con los proveedores de nube subyacentes. Es decir, si creo  un cluster de kubernetes en Compute Engine, este componente se hara cargo de incorporar funciones de Google Cloud, como Load Balancers y volumenes de almacenamiento
- Cada nodo ejecuta una pequeña familia de componentes del control plane denominado **kubelet**
- kubelet lo podes pensar como un agente del nodo
- Cuando un kube APIServer quiere iniciar un pod en un nodo, se conecta al kubelet de ese nodo
- Kubelet usa el tiempo de ejecucion del contenedor para iniciar el Pod y monitorea su ciclo de vida.
- El termino **Container runtime (tiempo de ejecucion del contenedor)** es el sw para iniciar un contenedor desde una imagen de contenedor.
- Luego esta **kube-proxy** que mantiene la conectividad de red entre los pods de un cluster


## GKE Autopilot vs GKE Standard
![[Pasted image 20251213165544.png]]

#### Autopilot
- Autopilot optimiza la administracion de Kubernetes con una experiencia practica
- Menos configuracion
- Solo pagamos lo que usamos
- crea clusteres de acuerdo con las practicas recomendadas
- define el tipo de maquina para tu cluster segun la carga de trabajo
- crea clusteres para produccion con mayor rapidez 
- Google ayuda a proteger los nodos del cluste
- reduce la superficie de ataque del cluster y errores continuos de configuracion
- monitorea todo el cluster entero
	- se asegura que los pods esten siempre programados
- es responsable de optimizar  el consumo de los recursos
#### Standard
- permite configurar la infraestructura de administracion
- esto significa mas sobrecarga de administracion, pero genera un control mas preciso
- pagamos por toda la infraesatructura aprovisionada


## Object Managment
![[Pasted image 20251213165722.png]]
- Debes saber que los objetos de kubernetes se identifican con nombre y un identificador unicos
- En kubernetes nosotros indicamos que objetos queremos, esto mediante un archivo manifiesto.

![[Pasted image 20251213170321.png]]
- Estos "manifestos" son archivos comunes que pueden escribirse en YAML o JSON
	- Este define el estado deseado de un Pod: un nombre y la imagen
- Los campos obligatorios del manifesto son:
	- apiVersion: indica la version de la API
	- kind: el tipo de objeto que queremos crear
	- metadata: identifica el nombre, id unico y un espacio de nombres opcional
- Si todos los objetos estan relacionados, lo mejor es definirlos en un mismo archivo YAML
- Se recomiendan gestionar en control de version los archivos YAML. Se puede usar Cloud Source Repositories
- Caracteristicas:
	- metadata.name: los objetos tienen un unico nombre y se acepta cualquier caracter
	- metadata.uid:  el identificador unico
	- labels: ayudan a identificar y organizar objetos.

![[Pasted image 20251213170746.png]]
- Una forma  de crear 3 servidores web de ngnix es declara 3 objetos de Pods. Cada uno con su propio file YAML

![[Pasted image 20251213171406.png]]
- En kubernetes, las cargas de trabajo se distribuyen equitativamente entre los nodos disponibles. Para ello necesaitamos garantizar la alta disponibilidad
	- Una opcion es usar **Object Controller**, que se encarga de gestionar el estado de los pods
	- como los pods estan diseñados para ser efimeras y descartables, no se reparan a si mismas y no se ejecutan por siempre
	- Si encuentra el controlador que un pod no esta en el estado deseado, la descarta e inicia otro pod


# Kubernetes Operations

## kube ctl command
![[Pasted image 20251213175203.png]]
- Se usa para comunicarse  con el servidor de la API de Kube en el "control plane".
- Es importante para los usuarios porque les permite enviar solicitudes al cluster y kubctl determina con que parte del plano de control comunicarse
- En un cluster, kubectl transforma las entradas de linea de comandos en llamadas a la API las envia al servidor de la API de Kube
- Sin embargo, para funcionar correctamente, kubectl se debe configurar con la ubicacion y las credenciales de un cluster de kubernetes.
- Se debe configurar kubectl para que se pueda usar y configurar un cluster
- kubectl almacena su configuracion en un archivo en el directorio principal, *en una carpeta llamnada .kube* que contiene las listas  de clusteres y credenciales

![[Pasted image 20251213175359.png]]
- El flujo es el siguiente, el admin quiere mandar un comando para obtener los pods
```bash
	kubectl get pods
```

- kubectl las procesa y hace una llamada a la APi "kube-apiserver"
- el "kube-apiserver" devuelve la respuesta al kubectl, el mismo la procesa nuevamente y se la manda al administrador.

![[Pasted image 20251213180140.png]]
Para conectar un cluster GKE con kubectl, debes recuperar las credenciales del cluster especifico
- para ello usa "get-credentials" de gcloud en cualquier otro entorno en el que hayas instalado las herramientas de linea de comandos de gcloud y kubect
	- Ten en cuenta que ambos se instalan de manera predeterminada en Cloud Shell
- El comando "get-credentials" de gcloud escribe, por defecto, la informacion de configuracion en un archivo **.kube** del directorio **$HOME**
	- Si se vuelve a ejecutar en un cluster distinto se actualizara el archivo de configuracion con las credenciales del cluster nuevo


#### kubectl syntax
![[Pasted image 20251213180632.png]]
Su sintaxis consta de 4 partes:
- **command**: especifica la accion que quieres hacer, como "get, describe, logs, exec, ..."
- **type**: define sobre que objetos actua el comando, como "pods, deployment, nodes"
- **name**: especifica el objeto que se definio en "type". Devuelve la informacion de un objeto mas especificamente
- **flags**: sirve para hacer request especiales