# Elastic Google Cloud Infraestructure - Scaling and Automation

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