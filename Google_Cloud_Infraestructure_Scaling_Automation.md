
# Interconnecting Networks
## Cloud VPN

- Google Cloud ofrece 2 tipos de gateways: 
	- HA VPN
	- *VPN Classic*: Se consideran gateawys de este tipo, cuando se crean antes de la *HA VPN*. Esta conecta de forma segura su red local a su red VPC de Google Cloud a traves de un tunel VPC IPSec. El trafico  que viaja entre las 2 redes se cifra mediante una gateway VPN y luego se descifra en la otra gateway VPN, esto protege la informacion que viaja a traves de internet publico y por eso la VPN Clasica es util para conexiones de datos de bajo volumen. Classic VPN ofrece un SLA de disponibilidad del servicio del 99,9% y admite:
			- VPN site to site
			- rutas estaticas y dinamicas
			- cifrados de IKEv1 y IKEv2 

#### Classic VPN Topology
![[Pasted image 20251117080650.png]]
- Este diagrama muestra una conexion VPN Classic entre su VPC y red local
- La red VPC se compone de subredes como us-west1 y us-east1
- Los recursos de la red VPC se pueden comunicar mediante la IP Interna porque el enrutamiento dentro de la red se configura automaticamente **(simpre las reglas de firewall por default permiten la comunicacion)**
- Luego se presenta un Cloud VPN Gateway en la red de VPC, el cual va a hacer uso de una IP Externa regional
- Por otro lado, La red local tambien debe tener una VPN Gateway (On Premise VPN Gateway) el cual puede ser un dispositivo fisico que exponga una IP Externa
- Un **tunel VPN** conecta sus gateways y sirve como un medio virtual a traves del cual se transmite el trafico cifrado
- Para establecer una conexion entre 2 gateways, debemos tener 2 tunels VPN
- Una cosa a considerar es que la MTU(Unidad de transmision maxima) para nuestra gateway VPN Local, no puede ser mayor a 1460 bytes debido al encapsulamiento y cifrado de los paquetes.


## HA VPN
- HA VPN es una solucion de VPN de la nube de alta disponibilidad que permite conectar de forma segura su Red Local a su Red VPC de la nube a traves de **una conexion VPN IPSec**
- HA VPN ofrece un SLA de disponibilidad del servicio del 99,9%
- Para llegar a este nivel de disponibilidad de SLA para las conexiones de alta disponibilidad, debemos configurar correctamente 2 o 4 tuneles desde su HA VPN gateway  a gateway VPN de pares o a otra gateway  VPN de alta disponibilidad
- Google Cloud automaticamente crea 2 IPS externas fijas cuando se crea la HA VPN gateway
- Los tuneles conectados a HA VPN deben usar **enrutamiento dinamico (BGP)**
- HA VPN soporta soporta VPN de sitio - sitio en una de las siguientes topologias:
	- HA VPN gateway a dispositivos VPN pares (VPN gateway to peer VPN Devices)
	- HA VPN gateway a una VPN gateway de AWS (VPN gateway to an Amazon Web Services virtual private gateway)
	- 2 HA VPN gateways conectadas entre si (Two HA VPN gateways connected to each other)

#### VPN gateway to peer VPN Devices topology
![[Pasted image 20251122112906.png]]
- Esta topologia dispone de  una VPN HA gateway conectado a 2 dispositivos VPN separados, cada uno con su propia direccion IP
- Tambien dispone de otra VPN HA gateway que conecta a una VPN de pares que usa solo una direccion IP
- Cada dispositivo par tiene una interfaz y una direccion IP Externa

#### VPN gateway to an Amazon Web Services virtual private gateway
![[Pasted image 20251122113737.png]]
- Al configurar una puerta de enlace  VPN externa HAq a  AWS. puede usar una **puerta de enlace de transito** o **pueta de enlace privada virtual** 
- la puerta de enlace de transito admite enrutamiento multirruta de igual costo (ECMP)
	- Cuando esta activo ECMP, entonces distribuye equitativamente el trafico en los tuneles


#### Two HA VPN gateways connected to each other
![[Pasted image 20251122114241.png]]
- Aca podemos conectar 2 redes  Google Cloud VPC entre si mediante una púerta de enlace VPN HA en cada red
- Desde la perspectiva de cada HA VPN gateway, se crean 2 tunnels
	- La interfaz 0 de una VPN gateway a la otra
	- La interfaz 1 de una VPN gateway a la otra 



## Cloud Interconnect and Peering
Hay diferentes servicios de interconexion y peering en la nube disponibles para conectar su infraestructura a la red de Google
![[Pasted image 20251122120803.png]]
- Estos servicios se pueden dividir en conexiones dedicadas o compartidas y ademas, conexiones de capa 2 o capa 3.
- La conexiones dedicadas proporcionan una conexion directa a la red de Google
- Las conexiones compartidas proporcionan una conexion a la red de Google a traves de un socio
- Las conexiones de capa 2 usan VLAN que se conecta directamente a su entorno GCP y proporciona conectividad a direcciones IP Internas en el espacio de direcciones RFC 1918
- Las conexiones de capa 3 brindan acceso de los servicios de Google Workspace, Youtube y las APIS de Google Cloud mediante direcciones IP publicas
- Cloud VPN es util para Direct Peering y Carrier Peering



## Cloud Interconnect
## Dedicated Interconnect
![[Pasted image 20251122120910.png]]
- La interconexion dedicada proporciona conexiones fisicas directas entre su red local ya la red  de Google
- Esto permite transferir grandes cantidades de datos entre las redes, lo que puede  resultar mas rentable que comprar ancho de banda adicional a traves de internet publico
- Para usarlo, necesitamos antes una conexion cruzada entre la red de Google y nuestro enrutador en **una instalacion de coubicacion comun**
- Para intercambiar las rutas entre las redes  configure una sesion BGP a traves de la interconexion entre el enrutador de la nube y el enrutador local
	- Esto permitira que el trafico de usuarios de la red local llegue a los recursos de  GCP en la red VPC y viceversa



![[Pasted image 20251122121343.png]]
- Para utilizar la interconexion dedicada, su red debe encontrarse fisicamente con la red de Google en una instalacion compatible
- En este mapa se puede observar las ubicaciones compatibles para las interconexiones: [https://cloud.google.com/interconnect/docs/concepts/colocation-facilities].


Si su red local no esta cerca de estas ubicaciones, puede optar por lo siguiente:
## Partner Interconnect
![[Pasted image 20251122123317.png]]
- Proporciona conexion entre la red local con la red VPC a traves de un **proveedor de servicios compatibles**
- Esto es util si su centro de datos esta en una ubicacion fisica que no puede acceder a una  instalacion de coubicacion de interconexion dedicada o si sus necesidades no estan justificadas en una interconexion dedicada
- Debemos trabajar entonces con un proveedor de servicios
- Estos proveedores tienen conexiones fisicas existentes a la red de Google que ponen a disposicion de sus clientes  para que las utilicen
- Luego debemos establecer una sesion BGP entre el enrutador de la nube con el enrutador local para empezar a pasar trafico entre tus redes a traves  de la red del proveedor de servicios
- Ver proveedores disponibles: https://cloud.google.com/interconnect/docs/concepts/service-providers

## Cross Cloud Interconnect
- Ayuda a establecer un conexion dedicada de alto ancho de banda entre Google Cloud y otro proveedor de servicios de la nube
- Cuando compramos esto, Google Cloud nos provee una conexion fisica dedicada entre la red de Google y la de otro proveedor en la nube
- Podemos usarlo cuando queremos conectar una VPC de GCP con una red alojada en otro proveedor de servicios de la nube
- Actualmente provee compatibilidad en: AWS, Azure y Oracle Cloud y Alibaba Cloud para su uso con Cross Cloud Interconnect
- Las conexiones de interconexion entre nubes estan disponibles en 2 tamaños: 10 Gbps o 100 Gbps

## Peering
Google permite  establecer una conexion Peering directo entre tu red empresarial y Google
- Con esta conexion podras intercambiar trafico de internet entre tu red y Google en una de las ubicaciones de red perimetral de amplio alcance de Google
- El peering directo con Google se realiza mediante el intercambio de rutas BGP entre Google y la entidad peering
- Una vez establecida al conexion de intercambio directo, puedes usarla para acceder a todos los servicios de Google incluido el conjunto completo de productos de Google Cloud
- No tiene SLA
- Para hacer uso de esto, necesita cumplir con los requisitos
- Los PoP(Puntos de presencias) perimetrales de Google Cloud son donde la red  de Google se conecta al resto de internet a traves del peering


![[Pasted image 20251122135143.png]]
- Los PoP estan a mas de 90 intercambios de internet y en mas de 100 instalaciones de interconexion en el mundo
- Si no estas cercas de estos puntos, entonces deberias optar por  **Carrier Peering**
	- Debemos conectarnos a traves de un socio de Carrier Peering
![[Pasted image 20251122135352.png]]



## Shared VPC
![[Pasted image 20251124123448.png]]
- La "VPC compartida" permite que una organizacion conecte recursos de multiples proyectos a una "red de nube privada virtual (VPC) comun" para que puedan comunicarse entre si de forma segura y eficiente mediante el uso de direcciones IP Internas de esa red.
- Cuando usamos Shared VPC, designamos un proyecto como **host** y le adjunto uno o mas **proyectos de servicio**
- Las redes VPC en el "proyecto host", se denominan **redes VPC compartidas**
- Los recursos de los "proyectos de servicio" púeden usar subredes en la red VPC compartida 
- La VPC compartida permite a los administradores de la organizacion delegar responsabilidades administrativas
- Un proyecto que esta en una Shared VPC, puede ser de host o de servicio
- El proyecto que no esta en una Shared VPC, es un proyecto independiente


## VPC Peering 
![[Pasted image 20251124124218.png]]
- El VPC Peering permite la conectividad privada RFC 1918 entre 2 redes VPC, sin importar si son del mismo proyecto o de la misma organizacion
- Para que el emparejamiento se establezca, el administrador de **la red del producer** debe emparejar la red del producer con la del consumidor, y el adminitrador **del consumer** debe emparejar la red del consumer con la red del productor 
- El VPC peering es un enfoque descentralizado o distribuido para la creacion de redes multiproyecto porque cada red VPC puede permanecer bajo el control de grupos administradores separados
- Los proyectos consideraran usar IP Externa o VPN para facilitar la comunicacion
![[Pasted image 20251124124351.png]]

# Load Balancing and AutoScaling

## Managed instance group
![[Pasted image 20251124130138.png]]
- Un grupo de instancias administradas es una coleccion de instancias de maquinas virtuales identicas, que usted controla como  una sola entidad mediante una plantilla de instancias
- Los grupos de instancias pueden trabajar con Load Balancing para distribuir el trafico de red a todas las instancias del grupo
- Si una instancia del grupo se detiene, bloquea o elimina entonces "el grupo de instancias" recrea otro, esta usara el mismo nombre y la misma plantilla.
- Se recomiendan **los grupos de instancias regionales**, porque permiten distribuir la carga en varias zonas en lugar de limitar la aplicacion en una zona

**Guia para crear un grupo de instancias:**
![[Pasted image 20251124130420.png]]


## AutoScaling and Health Checks
![[Pasted image 20251124132405.png]]
- Los grupos de instancias administradas ofrecen capacidades de escalamiento automatico que le permiten: agregar o eliminar automaticamente instancias de un grupo de instancias administradas en funcion del aumento de trafico
- El escalamiento automatico ayuda a que sus  aplicaciones gestionen con elegancia los aumentos en el trafico y reduce los costos cuando la necesidad del recurso es menor 
- Simplemente debemos definir la politica de escalamiento automatico y el AutoScaling realizara un escalamiento automatico en funcion al trafico


#### Health Check
![[Pasted image 20251124133326.png]]
- Un health check o verificacion de estado es muy similar a una verificacion de tiempo de actividad en Cloud Monitoring
- Simplemente define un protocolo, un puerto y criterios de salud, como se muestra en esta captura de pantalla
- En funcion a la configuracion de arriba, Google Cloud calcula un estado de salud para cada instancia
- Los criterios de salud definen la frecuencia con la que se debe verificar si una instancia esta en buen estado (intervalo de verificacion); cuanto tiempo esperar una respuesta (ese es el tiempo de espera); intentos exitosos (umbral saludable)-; y cuantos intentos fallidos son decisivos (umbral insalubre)


## External Application Load Balancer
![[Pasted image 20251124135704.png]]
- Los Load balancers  externos se implementan mediante front-ends de Google (GFE) o servidores proxy administrados. 
- Los load balancers externos globales y los Load Balancers Clasicos usan GFE
- Los GFE ofrecen equilibrio de carga  en multiples regiones en el nivel Premium, dirigiendo el trafico al healthy backend mas cercano
- Los load balancers externos globles y regionales usan el software **proxy Envoy** de codigo abierto para habilitar las capacidades avanzadas de gestion del trafico
	- Este tipo de load balancers se puede aplicar a distintos modos: Global, Regional y Classic


Ahora vamos a explicar un poco mas detalle la arquitectura:
![[Pasted image 20251124140120.png]]
- Una regla de reenvio externo especifica una direccion IP Externa, un puerto y un proxy HTTP(s)
- Los clientes usan la direccion IP y el puerto para conectarse al balanceador de carga
- Un proxy HTTP(s) de destino recibe una solicitud del cliente, y la procesa.
- El proxy tambien puede autenticar las comunicaciones mediante el uso de certificaciones SSL
- Un servicio de backend distribuye solicitudes a backends healthy o saludables.


## Cloud CDN
![[Pasted image 20251130193648.png]]
- Usa los PoP (Puntos de presencia) perimetrados distribuido globalmente de Google para almacenar en cache contenido de equilibrio de carga HTTP(s) cerca de sus usuarios
- El contenido se puede almacenar en cache en los nodos CDN
- Cloud CDN almacena en cache el contenido en los bordes de la red de Google, lo que proporcioan una entrega mas rapida de contenido a sus usuarios y al mismo tiempo reduce los costos de servicio
- Podemos activarlo solo mediante un checkbox al configurar el servicio de backend de su "Application Load Balancer"
- Podemos usar los modos de caches para controlar factores que determinan si  Cloud CDN almcena en cache o no su contenido mediante los modos que ofrece
- Los modos que ofrece son:
	- USE_ORIGIN_HEADERS: requiere respuestas de origen para establecer directivas de cache validas y encabezados de almacenamiento en cache validos
	- CACHE_ALL_STATIC: almacena en cache automaticamente el contenido estatico que no tiene la directiva no-store, private o no-cache
	- FORCE_CACHE_ALL: almacena en cache las respuestas de forma incondicional, anulando cualquier directiva de cache establecida por el origen

## Network Load Balancing
![[Pasted image 20251130194608.png]]
- Los "Network load balancing" son balanceadores de carga de capa 4 que pueden manejar trafico TCP, UDP u otro protocolo IP.
- Estan disponibles como balanceadores de carga proxy o passtrhough
- Tipos de Balancers:
	- Proxy: elegirlo si desea configurar un balanceador de proxy inverso con soporte para controles de trafico avanzados y backend locales y en otros entornos de la nube
	- Passtrhough: elegirlo si deseamos preservar la direccion IP de origen de paquetes de los clientes

#### Arquitectura de un Proxy Network Load Balancer
![[Pasted image 20251130195158.png]]
- son de capa 4
- distribuyen el trafico TCP a las VMs en su red VPC de Google Cloud
- el trafico finaliza en la capa de equilibrio de carga y luego se reenvia al backend mas cercano mediante TCP
- se pueden implementar de manera externa o interna
- estos balanceadores estan construidos sobre front-ends de Google (GSE) o servidores proxys Envoy
- Se pueden configurar en modos como: global, regional o classic

## Arquitectura de Passtrhough Network Load Balancer
![[Pasted image 20251130195937.png]]
- son de capa 4
- estos balanceadores distribuyen el trafico entre los backends de la misma region que el balanceador de carga 
- se implementan mediante el uso de redes virtuales Andromeda y Google Maglev
- no son proxies
- reciben los paquetes con equilibrio de carga con las direcciones IP de origen y destino del paquete, el protocolo y si el protocolo esta basado en el puerto
- las respuestas de las VMs del backend van directamente al cliente, no regresan a traves del load balancer (DSR - Direct server return)



## Internal Load Balancing
![[Pasted image 20251130203006.png]]
- Son balanceadores de carga  de capa 7 regionales basados en proxy Envoy 
- permite ejecutar y escalar el trafico de su aplicacion HTTP detras de una direccion IP interna
- los balanceadores de carga de aplicaciones internas permiten backends en una region pero pueden configurarse para que los clientes puedan acceder a ellos gloablmente desde cualquier region de Google Cloud
- optimizan la distribucionj del trafico dentro de su red VPC o redes  conectadas a su red VPC
- pueden configurarse en 2 modos:
	- **regional:** garantiza que todos los clientes y los backends sean de uan misma region
	- **interregion:** permite equilibrar la carga de trafico hacia servicios backend que estan distribuidos, tambien garantiza que el trafico se rediriga al backend mas cercano

#### Internal Passtrhough Network Load Balancers

- es un servicio de equilibrio de carga privado regional para cuando necesitas equilibrar la carga de trafico TCP, UDP, ICMP, ICMPv6, SCTP, ESP, AH y GRE. O sino cuando necesitamos equilibrar la carga de un puerto TCP
- permite ejecutar y escalar sus servicios detras de una direccion IP privada de balanceo de carga
- las solicitudes de sus clientes internos permanecen internas en su red y region VPC

#### Internal Proxy Network Load Balancers
- es un balanceador impulsado por el software de proxy de codigo abierto Envoy y la pila de virtualizacion de red Antromeda
- equilibra la carga de trafico dentro de su red VPC o redes conectadas a su red VPC
- es de capa 4
- estan disponibles  en internos regionales o internos entre regiones



# Infraestructure Automation

## Terraform
![[Pasted image 20251201232404.png]]
- Es una de las herramientas usadas para infraestructura como Codigo o IaC
- La "Infraestructure as Code" permite  el rapido aprovisionamiento y eliminacion de infraestructuras
- El aprovisionamiento bajo demanda de una implementacion es extremadamente poderoso. Esto se puede integrar en un proceso de integracion continua que facilita el camino hacia la implementacion continua. 
- El aprovisionamiento automatizado de infraestructura significa que la infraestructura se puede aprovisionar a demanda y la complejidad de la implementacion se administra en el codigo. Esto permite **cambiar la infraestructura** a medida que cambian los requisitos
- La infraestructura para entornos como desarrollo y prueba ahora puede replicar facilmente la produccion y eliminarse inmediatamente cuando no este en uso. Todo esto **gracias a la infraestructura como codigo**
- Podemos usar varias herramientas para IaC. Google Cloud admite Terraform, donde las implementaciones se describen en un archivo conocido como configuracion (Aqui se detallan todos los recursos que deben aprovisionarse)
	- Las configuraciones se pueden  modularizar  usando plantillas  que permiten la abstraccion de recursos en componentes reusables en todas las implementaciones
- **Ademas de Terraform,** Google Cloud tambien proporciona soporte a otras herramientas IaC, entre ellas estan "Chef, Puppet, Ansible y Packer"



![[Pasted image 20251201232429.png]]
- Aca nos centraremos en **Terraform**
- Terraform le permite aprovisionar recursos en Google Cloud (como maquinas virtuales, contenedores, almacenamiento y redes) con archivos de **1configuracion declarativos**. 
	- El lenguaje de configuracion **HashiCorp** (HCL) permite descripciones concisas de recursos usando bloques, argumentos y expresiones
	- La implementacion se puede repetir, y podemos eliminarla con un solo clic o comando
	- La ventaja del **enfoque declarativo** es que permite especificar cual debe ser la configuracion y dejar que el sistema determine los pasos a seguir
- En lugar de cada recurso por separado, **usted especifica el conjunto de recursos que componen la aplicacion**
- Terraform usa las API subyacentes  de cada servicio de Google Cloud, para implementar sus recursos

#### Terraform Language
![[Pasted image 20251201233647.png]]
- Es la interfaz  de usuario para declara los recursos
- Los recursos son objetos de infraestructura como maquinas virtuales de Compute Engine, despositos de almacenamiento, contenedores o redes.
- Una configuracion en terraform es un documento en **lenguaje Terraform** que le indica a Terraform como administrar una coleccion determinada de infraestructura.
	- Una configuracion puede constar de varios archivos y directorios
- La sintaxis del lenguaje de Terraform se compone:
	- Bloques que representan objetos y pueden tener cero o mas etiquetas
	- Un bloque tiene un cuerpo que le permite declarar argumentos y bloques anidados
	- Los **argumentos** se usa para asignar un valor a un nombre
	- Una **expresion** representa un valor que se puede asignar a un identificador 


#### Ejemplo con Terraform
![[Pasted image 20251201234314.png]]
![[Pasted image 20251201234335.png]]
- Para el ejemplo vamos a definir una archivo **main.tf**
- A medida que nuestra infraestructura se vuelve mas compleja podemos construir cada elemento en un archivo separado
- Comenzemos con **main.tf**, aca es donde especificamos la infraestrucutra que deseamos crear. Es como un plano del estado que deseamos
	- Primero definimos el provider
- Luego definimos  nuestra **red**
	- dejamos el "auto_create_subnetworks" como true, eso provocara que se cree una subred por cada region
	- luego el **mtu** en 1460
- Luego definimos nuestro **firewall**, donde permitimos trafico TCP al puesto 80 / 8080
- una vez que completamos de definir el archivo, podemos crear la infraestructura en el Cloud shell mediante el comando 
```bash
# inicializa la nueva configuracion de Terraform
# la ejecutamos en el directorio del archivo main.tf
terraform init

# realiza una actualzacion
terraform plan

# crea la infraestructura definida en el archivo main.tf
terraform apply
```

## Lab: Automating the Deployment of Infrastructure Using Terraform

Terraform te permite crear, cambiar y mejorar infraestructura de forma segura y predecible. Es una herramienta de código abierto que codifica las API en archivos de configuración declarativa que se pueden compartir entre los miembros de un equipo, tratar como código, editar, revisar o cambiar de versión.

En este lab, crearás una configuración de Terraform con un módulo para automatizar la implementación de la infraestructura de Google Cloud. En particular, implementarás una red de modo automático con una regla de firewall y dos instancias de VM, como se muestra en este diagrama:

```bash
# Verificamos que se instalo correctamente terraform
terraform -version

# Creamos un folder donde estara nuestros archivos de configuracion
mkdir tfinfra
cd tfinfra

nano provider.tf
'''
provider "google" {}
'''

# iniciamows terraform
terraform init

nano mynetwork.tf
'''
# Create the mynetwork network
resource "google_compute_network" "mynetwork" {
  name = "mynetwork"
  # RESOURCE properties go here
  auto_create_subnetworks = true
}

# Add a firewall rule to allow HTTP, SSH, RDP and ICMP traffic on mynetwork
resource "google_compute_firewall" "mynetwork-allow-http-ssh-rdp-icmp" {
  name = "mynetwork-allow-http-ssh-rdp-icmp"
  # RESOURCE properties go here
  
  #Indica esta proipiedad que este recurso depende que se cree primero la red
  network = google_compute_network.mynetwork.self_link
  allow {
    protocol = "tcp"
    ports    = ["22", "80", "3389"]
  }
  allow {
    protocol = "icmp"
  }
  source_ranges = ["0.0.0.0/0"]
}

# Create the mynet-vm-1 instance
module "mynet-vm-1" {
  source           = "./instance"
  instance_name    = "mynet-vm-1"
  instance_zone    = "us-east4-a"
  instance_network = google_compute_network.mynetwork.self_link
}

# Create the mynet-vm-2" instance
module "mynet-vm-2" {
  source           = "./instance"
  instance_name    = "mynet-vm-2"
  instance_zone    = "europe-west1-c"
  instance_network = google_compute_network.mynetwork.self_link
}
'''
# Nota: La configuracion de network.tf define la red, firewalls y las VMS a ejecutarse

# Creamos un folder nuevo para las VMs
mkdir instance
nano main.tf
'''
resource "google_compute_instance" "vm_instance" {
  name = "${var.instance_name}"
  # RESOURCE properties go here
  zone         = "${var.instance_zone}"
  machine_type = "${var.instance_type}"
  boot_disk {
  initialize_params {
      image = "debian-cloud/debian-11"
     }
  }
  network_interface {
    network = "${var.instance_network}"
    access_config {
      # Allocate a one-to-one NAT IP to the instance
    }
  }
}
'''
# Nota: Las vamos a hacer parametizables la creacion de las VMs

nano variables.tf
'''
variable "instance_name" {}
variable "instance_zone" {}
variable "instance_type" {
  default = "e2-micro"
  }
variable "instance_network" {}
'''
# Nota: definimos las variables de entrada para las VMs, como opcional el type instance va a tener por default "e2-micro"


# Le damos formato a los archivos .tf
terraform fmt

terraform init

# Creamos el plan de ejecucion
terraform plan

# Aplicamos los cambios "yes"
terraform apply
```


## Google Cloud Marketplace
- Permite implementar rapidamente paquetes de software funcionales que se ejecutan en Google Cloud
- Ofrece soluciones de nivel de produccion de proveedores externos que ya han creado sus configuraciones con Terraform. Estas soluciones se facturan junto con todos los servicios de Google Cloud de su proyecto
- Google Cloud actualiza las imagenes de esos paquetes de SW para solucionar problemas criticos y vulnerabilidades

# Managed Services

## BigQuery
![[Pasted image 20251202225814.png]]
- Es un almacen de datos en la nube sin servidor (serverless), altamente escalable y rentable en Google Cloud
- Es un almacen de datos a escala de petabytes que permite realizar consultas ultrarrapidas usando la potencia de procesamiento de la infraestructura de Google
- Es usado para todo tipo de organizaciones


## Dataflow
- Es un servicio administrado para ejecutar una amplia variedad de patrones de procesamiento de datos
- Es esencialmente un servicio para transformar y enriquecer datos en **batch y stream processing**
- Se escala automaticamente para satisfacer las demandas de sus pipelines
- Permite escalar de manera inteligente a millones de consultas por segundo
- Dataflow admite un desarrollo de canalizacion rapido y simplifcado a traves de la API expresivas de SQL, Java y Python en el SDK de Apache Beam
- Dataflow esta estrechamente vinculado con otros servicios de Google Cloud como **Google Cloud Observability** por ende podemos monitorear su canalizacion y la calidad de los datos, que entran y salen.

![[Pasted image 20251202231255.png]]

- Como vimos, los datos pueden procesarse en stream o batchs
- Los datos pueden provenir  de otros servicios de Google Cloud como Datastore o Pub/Sub
- Tambien los datos pueden provenir de servicios terceros, como **Apache Avro o Apache Kafka**
- Despues de transformar los datos con Dataflow, puedes analizarlo por BigQuery, Vertex IA o Bigtable
- Con Looker Studio podemos crear paneles de control en tiempo real para dispositivos IoT


## DataPrep
![[Pasted image 20251203075430.png]]
- Es un servicio de datos inteligentes para explorar, limpiar y preparar visualmnete datos estructurados y no estructurados para analisis, informes y aprendizaje automatico
- Es serverless
- Con esquema automatico, tipo de datos, posible uniones y deteccion de anomalias puede omitir la elaboracion de perfiles de datos que requiere mucho tiempo y concentrarse en el analisis de datos
- Dataprep es un servicio de socio integrado operado por Trifacta y basado en su solucion de preparacion de datos lider en la industria Trifacta Wangler
- Google trabaja con Trifacta par brindar una buena experencia de usuario, ya que elimina la necesidad de instalacion de SW inicial, costos de licencia separado o gastos operativos
- Dataprepn se escala segun al demanda para satisfacer sus crecientes necesidades de preparacion de datos, para que pueda centrarse en el analisis

## DataProc
![[Pasted image 20251203080308.png]]
- Es un servicio de la nube rapido, facil de usar y totalmente administrado para ejecutar clusteres  Apache Spark y Apache Hadoop de una manera sencilla
- solo pagamos los recursos que usas mediante la facturacion por segundo
	- Si aprovecha **instancias preemptibles** en su cluster, puede reducir aun mas su costo
- Sin usar Dataproc puede llevar entre 5 - 30 minutos crear clusteres Spark y Hadoop
- Esta integrado a otros servicios de Google Cloud como **BigQuery, Cloud Storage, BigTable, Cloud Logging y Cloud Monitoring**
- Como servicio administrado puede crear clusteres rapidamente, administrarlos facilmente y ahorrar dinero cuando no se lo necesite