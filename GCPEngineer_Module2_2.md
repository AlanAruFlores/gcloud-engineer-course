# GCP Engineer - Core Infraestructure (Module 2 - Part 2)

## Cloud Storage

![[Pasted image 20251030224010.png]]

- Ofrece un almacenamiento de objetos durable y con alta disponibilidad
- El **almacenamiento de objetos** es una arquitectura de almacenamiento de datos que los administra como objetos
- Estos objetos se almacenan en un formato empaquetado que contiene **forma binaria** de los datos reales como los metadatos.
- Entre los datos se incluyen: videos, imagenes y grabaciones de audio
- **Cloud Storage** es el servicio de almacenamiento de objetos
- Suele usarse para almacenar grandes objetos binarios (BLOB)
- Se organizan en buckets. El cual un bucket debe tener un nombre y su ubicacion puede afectar la latencia de la misma.



![[Pasted image 20251030224301.png]]
- Los objetos no se pueden modificar, son inmutables, sino que se crean versiones de la misma
- Las versiones nuevas reemplazaran las viejas
- El control de versiones permite ver versiones archivadas del objeto

### Tipos principales en Cloud Storage
![[Pasted image 20251030230356.png]]
- **Standard Storage:** es ideal para datos de acceso frecuente o "activos". Tambien para datos que se almacenan durante periodos breves.
- **Nearline Storage**: ideal para almacenar datos de acceso poco frecuente, como leer o modificar un datos al mes. Ejemplo: copia de seguridad, contenido multimedia larga o datos archivados.
- **Coldline Storage:** es una opcion de bajo costo para datos de acceso poco frecuente, a diferncia de Nearline, esta diseñada para leer una vez cada 90 dias.
- **Archive Storage:** es la de menor costo y se usa para archivar datos, copias de seguridad en linea y la recuperacion de datos ante desastres. Es ideal para el acceso a los datos 1 vez al año.


### AutoClass
![[Pasted image 20251030231145.png]]
- Cloud Storage tambien ofrece una nueva herramienta denominada **AutoClass**, el cual es una funcion que traslada automaticamente los objetos a la clase de almacenamiento ideal segun sus patrones de acceso.
- Los datos menos accedidos los traslada automaticamente en el **coldline storage**
- Los datos mas accedidos los traslada automaticamente en la **standard storage**
- Automatiz el ahorro de costos
- No tiene tarifa minima porque pagas por su uso
- El servidor encripta los datos antes de escribirlos en el disco.
- Los **datos del lado del cliente** se encriptan en HTTPS/TLS (capa de transporte
- Existen varias maneras de transferir la informacion:
	- podemos usar gcloud o el mismo Google Console para transferir la informacion.
	- Si queremos importar terabytes de informacion tenemos **"Storage Transfer Service"** que permite hacerlo de forma rapida y economica. Este servicio permite programar y administrar transferencias por lotes para Cloud Storage
	- Tambien tenemos **Transfer Appliance**, un servidor de almacenamiento de bastidores y de alta capacidad que puedes arrendar de Google Cloud. Podemos transferir hasta un *Petabyte*

## Cloud SQL
![[Pasted image 20251030232407.png]]
- Ofrece bases de datos relacionales administradas con MYSQL, PostgreSQL y SQL Server como servicio.
- Puede escalar hasta 128 nucleos de procesamiento, 864 GB de RAM y 64TB de almacenamiento.
- Usa **replicacion automatica**
- Admite copias de seguridad
- Incluye firewall de red, que controla el acceso de red a instancias de la bases de datos


## Spanner
![[Pasted image 20251030232456.png]]
- Spanner es un servicio administrado de bases de datos relacionales que escala de forma de manera horizontal.
- Funciona con SQL
- Spanner es el servicio que impulsa el negocio de la empresa de Google por $80000 millones
- Es adecuado para alta disponibilidad, coherencia global y una gran cantidad de operaciones por segundos.

## FireStore
![[Pasted image 20251030232810.png]]
- FireStore es una base de datos en la nube NoSQL para desarrollar  soluciones moviles, web y de servidores.
- Aca los datos se organizan en formato de documentos y se organizan en colecciones
- Los documentos pueden contener objetos anidados complejos, ademas de subcolecciones.
- Incluyen filtros y ordenamiento
- Almacena ademas en cache de los datos que usa la App de modo que pueda **escribir, leer,  escuchar y consultar los datos**

## BigTable
![[Pasted image 20251030233204.png]]

- **BigTable** es el servicio de bases de datos NoSQL de Google.
- puede administrar grandes cargas de trabajo con baja latencia y alta capacidad de procesamiento
- Es ideal para apps de operativas y analiticas. (analisis de usuarios, analisis financieros, IoT)
- Los datos pueden trasmitirse con frameworks de **streaming** populares como  Dataflow, Spark Streaming y Storm
- Tambien por **procesamiento por lotes** como: Hadoop, Dataflow o Spark.



## Containers
![[Pasted image 20251101150424.png]]

- Los IaaS permiten compartir recursos de procesamiento con otros desarrolladores usando maquinas virtuales para virtualizar hardware
- Un **contenedor** brinda la escalabilidad independiente de las cargas de trabajo en PaaS y  una capa de abstraccion del S.O y el hardware en IaaS
- Un **sistema configurable** te permite instalar tu entorno de ejecucion, servidor web , BD o middleware, configurar recursos (espacio, disco, etc.)
- Un contenedor:
	- es una caja invisible que rodea el codigo y sus dependencias
	- tiene acceso limitado a su propia particion del sistema de archivos y hardware
	- lo que se necesita en cada host es un kernel de S.O que admita contenedores
	- Escala como la PaaS ,pero brinda la misma Flexibilidad de las IaaS

![[Pasted image 20251101150547.png]]
- Cada contenedor va a poder escalar automaticamente los hosts.
- Sin embargo es usado cada contenedor como un microservicio, esto hace que sean modulares, mas faciles de implementar y escalables.


## Kubernetes
![[Pasted image 20251101160222.png]]
- **Kubernetes** ayuda a administrar y escalar aplicaciones alojadas en contenedores esto con el fin de ahorrar tiempo y esfuerzo cuando escalas  apps y cargas de trabajo.
- Kubernetes puede iniciarse con GKE (Google Kubernetes Engine)
- Kubernetes:
	- es una plataforma de codigo abierto para administrar cargas de trabajo y servicios en contenedores
	- permite orquestarlos o organizarlos muchos contenedores de muchos hosts, escalar como microservicios e implementar lanzamientos y reversiones.
	- es un conjunto de APIs que puede usar para implementar contenedores en un conjunto de nodos, denominado **cluster**
	- un **nodo** es una instancia de procesamiento , como una computadora o maquina.
	- Podemos describir un conjunto de apps y como deberian interactuar entre si.

![[Pasted image 20251101151545.png]]
- Implementar contenedores en nodos , se lo denomina un **Pod**
- **Pod** es la unidad mas pequeña en Kubernetes que puedes crear o implementar. Representaria un proceso en ejecucion en tu cluster  como un componente de tu app o una app entera.


![[Pasted image 20251101152049.png]]
- Por lo general tenes un contenedor por Pod, pero si tenes varios contenedores con una dependencia obligatoria puedes empaquetarlos en un solo Pod y compartir recursos de red entre ellos.
- El Pod proporciona una IP de red unica y un conjunto de puertos para tus contenedores y opciones que rigen como deben ejecutarse los contenedores.
- Para ejecutar un Pod de Kubernetes usamos el comando **kubectl** (inicia un objeto Deployment con un contenedor que se ejecuta en un Pod)
	- Un **Deployment** es un grupo de replicas del mismo Pod y mantiene tus Pods en ejecucion incluso en los nodos que fallan.
	- Este Deployment representa un componente de la app o incluso una app completa
- Para ver la lista de Pods podes ejectuar **kubectl get pods**

![[Pasted image 20251101154709.png]]
- Kubernetes CREA un **Service** con una IP FIJA para tus pods y un **Controlador** que indica "debo conectar un balanceador de cargas externo con una IP publica a ese Service"
	- 		- ![[Pasted image 20251101154835.png]]
- En GKE, el balanceador de cargas es de tipo RED
- Cualquier cliente que acceda a esa IP se enrutara al Pod del Service
- Un **Service** es una abstraccion que define un conjunto logico de Pods y una politica para acceder a ellos
	- ![[Pasted image 20251101155101.png]]
- Los deploymentes crean/destruyen Pods, y los pods tiene una IP que no se mantiene estable durante todo el tiempo
- Para escalar un Deployment , podemos ejecutar el comando 
```bash
kubectl scale
```


![[Pasted image 20251101155611.png]]
- La verdadera fortaleza de kubernetes surge cuando trabajas de forma declarativa, no emites comandos pero **provees un archivo de configuracion** que indica a Kubernetes como quieres que se vea tu estado deseado.
	- Esto se logra con un archivo de configuracion del Deployment
- Para ver que se esten ejecutando tu deployment con las replicas adecuadas puedes ejecutar 
```bash
	kubectl get deployments 
	# o
	kubectl describe deployments  
```

- Si necesitas actualizar un contenedor para ofrecer una nueva experiencia al ususario puedes usar (luego de esto, se crearan nuevos Pods segun tu nueva estrategia de actualizacion):
```bash
	kubectl rollout 
	# o
	kubectl apply
```


## Google Kubernetes Engine
![[Pasted image 20251101160412.png]]
- GKE es un servicio administrado de Kubernetes alojado por Google en la nube
- El entorno de GKE consta de varias maquinas, en particular,  instancias de Compute Engine que se agrupan para formar un cluster.

![[Pasted image 20251101161121.png]]
- GKE nos facilita ya que :
	- administra por nosotros todos los componentes del plano de control
	- expone una direccion IP a la que enviamos las solicitudes a la API de kubernetes.
	- es responsable de aprovisionar y administrar todo el plano de control.
- La configuracion y administracion de Nodos dependen del tipo de modo de **GKE** que uses:
	- Con el modo **Autopilot (recomendado)** GKE administra la infraestructura subyacente como la configuracion de nodos, el escalado y la actualizacion automatica, las configuraciones de seguridad y redes.
	- Con el modo **Standard** , aca administramos la infraestructura, incluida la configuracion de los nodos individuales.


- Para iniciar Kubenertes en un cluster de GKE, ejecutamos el siguiente comando
```bash
gcloud container clusters create k1
```


## Cloud Run
![[Pasted image 20251101162732.png]]
- Es una plataforma de procesamiento administrada que ejecuta contenedores serverless (sin estado) a traves de eventos Pub/Sub
- Es una tecnologia sin servidores
- No admiinistra la infra sino que debes solo enfocarte en el codigo.
- Se basa en **Knative**, una API abierta y un entorno de ejecucion basado en Kubernetes.
- Puede administrarse en Google Cloud, Kubernetes Engine o cualquier plataforma.


![[Pasted image 20251101162930.png]]
- El flujo de trabajo para desarrolladores se compone de 3 simples pasos:
	- Escribir tu codigo con tu lenguaje de programacion favorito.
	- Buildear y empaquetar tu aplicacion en una imagen del contenedor
	- Deployr a Cloud Run

#### Cloud Run Functions
![[Pasted image 20251101163350.png]]
- Puedes escribir una funcion con un proposito
- es una solucion asincrona ligera
- crean funciones de un proposito que responden a un evento
- se puede usar para conectar y extender servicios de nube
- Los eventos de Cloud Storage o Pub/Sub activan Cloud Run Functions asincronamente



## Prompt Engineering

### IA Generativa
![[Pasted image 20251101164422.png]]
- Es un subconjunto de la inteligencia artificial capaz de crear texto, imagenes y otros datos con modelos generativos a menudos en respuestas de PROMPTS
- Un PROMPT es una intruccion, indicacion o pregunta que se le da a un programa.

### LLMs

![[Pasted image 20251101164930.png]]
Los modelos de lenguajes grandes son modelos de lenguaje de uso general que se pueden entrenar previamente para fines especificos 
- Grande en este contexto se refiere a la GRAN CANTIDAD DE DATOS de entrenamiento, tambien a los PARAMETROS
Puede dar respuestas erroneas debido a la falta de entrenamiento , donminadas alucinaciones pero se puede solucionar esto mediante el PROMPT Engineering

### Promp Engineering
![[Pasted image 20251101165455.png]]
- El Prompt es el texto que se entrega al modelo
- El Prompt Engineering es la forma de articular tus instrucciones  para obtener una mejor respuesta del modelo

![[Pasted image 20251101165919.png]]
![[Pasted image 20251101165940.png]]
Tiene varios tipos de prompts:
	- Zero-shot prompt: No brindan ningun ejemplo o contexto de la instruccion
	- One-shot prompt: brindan un ejemplo en la instruccion
	- Few-shot promt: brindan mas de un ejemplo en la instruccion
	- Role Prompting: brinda referencias para que el modelo use cuando responda preguntas.


#### Elementos de un Prompt
![[Pasted image 20251101170158.png]]
- **Preambulo:** se refiere al texto de introduccion que proporcionas para dar instrucciones y contexto al modelo antes de nuestra pregunta o peticion
- **Input:** esta es la peticion central que hacemos al LLM