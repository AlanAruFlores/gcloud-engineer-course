
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
- 