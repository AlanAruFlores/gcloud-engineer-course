
## Maneras de usar Google Cloud
![[Pasted image 20251102122734.png]]
- Existen 4 maneras de usar Google Cloud
- **Google Cloud Console**: proporciona una interfaz grafica web. Aca podremos ver la informacion de los servicios, tambien la cantidad de instancias de una VM , etc.
- **Google Cloud CLI**: Si en lugar de la interfaz web, queremos trabajar con una terminal esta es la opcion. Podemos ejecutar comandos gcloud para gestionar los recursos.
![[Pasted image 20251102123047.png]]
- **REST based API:** Ademas del CLI de Google Cloud, tambien podemos usar las bibliotecas de cliente que permiten crear y administrar recursos facilmente. Las bibliotecas de cliente de Google Cloud exponen API con 2 propositos generales:
	- Las API de las aplicaciones  proporcionan acceso a los servicios y estan optimizadas para los lenguajes compatibles
	- Las API de administracion ofrecen funcionalidades para la gestion de recursos
![[Pasted image 20251102123640.png]]
- **Cloud Mobile App**: es otra forma  de interactuar con Google Cloud, permite gestionar los recursos de Google Cloud desde nuestro dispositivo Android o IOS



## Lab: Cómo trabajar con la consola de Google Cloud y Cloud Shell
```bash

	
 # CREAMOS UN BUCKET EN GOOGLE CLOUD
 export BUCKET_NAME = "bucket-alan"
 gcloud storage buckets create gs://$BUCKET_NAME 
 # Nota: podemos usar gsutil mb gs//BUCKET_NAME
 
 
 
 # MANDAMOS EL FILE AL BUCKET
 export FILE='GCP Plan de estudios.pdf'
 gcloud storage cp $FILE gs://alanaru2-bucket
# Nota: podemos usar gsutil cp $FILE gs://alanaru2-bucket

 # VEMOS LAS REGIONES 
 gcloud compute regions list
 
 # CREAMOS UN ARCHIVO DE CONFIGURACION PARA UN ESTADO PERSISTENTE EN LAS SHELLS
 mkdir infraclass
 touch infraclass/config
 
 export $VARIABLE=valor
echo VARIABLE=$VARIABLE >> ~/infraclass/config
 
 # PERMITE QUE EN OTRA INSTANCIA SHELL, SE GUARDEN LAS VARIABLES DE ENTORNO DEIFNIDOS EN EL ARCHIVO CONFIG de infraclass
 nano .profile
 source infraclass/config
```

**Notas:**
- **Cloud Shell proporciona lo siguiente:**
	- VM temporal de Compute Engine
	- Acceso mediante línea de comandos a la instancia a través de un navegador
	- 5 GB de almacenamiento en disco persistente ($HOME dir)
	- SDK de Cloud y otras herramientas preinstaladas
	- gcloud: para trabajar con Compute Engine y muchos servicios de Google Cloud
	- gcloud storage: para trabajar con Cloud Storage
	- kubectl: para trabajar con Google Kubernetes Engine y Kubernetes
	- bq (para trabajar con BigQuery)
	- Compatibilidad con los lenguajes Java, Go, Python, Node.js, PHP y Ruby
	- Función de vista previa en la Web
	- Autorización integrada para acceder a instancias y recursos



## Lab: Vista previa de la infraestructura
### Inicia Jenkins

1. Haz clic en **Comenzar**.
2. Verifica la implementación, acepta las condiciones y los servicios, y haz clic en **Aceptar**.
3. En la ventana emergente `Se aceptaron correctamente las condiciones`, haz clic en **Implementar**.
4. Si se te solicita, haz clic en **Habilitar** para la API de Compute Engine y la API de Infrastructure Manager.
5. En la página Implementación, en **Deployment Service Account**, selecciona **Cuenta existente** y elige la cuenta de servicio predeterminada de Compute Engine `service account`.
6. Selecciona la zona como `ZONE`.
7. En **Tipo de máquina**, selecciona **E2** como la **Serie** y **e2-standard-2 (2 vCPU, 1 core, 8 GB memory)** como el **Tipo de máquina**.
8. Deja todo con su configuración predeterminada y haz clic en **Implementar**.

### Revisa el software instalado y accede a Jenkins

1. En la pestaña **Details**, anota los valores de **Admin user** y **Admin password (Temporary)**, y agrégalos a un editor de texto.
2. Haz clic en el vínculo junto a **Site URL** para ver el sitio en otra pestaña del navegador. Si ves un mensaje de error, tal vez debas volver a cargar la página un par de veces.
3. Si se te solicita, haz clic en **Continue to site**.
### Explora Jenkins

1. En el panel izquierdo de la interfaz de Jenkins, haz clic en **Manage Jenkins**. Consulta todas las acciones disponibles. Ya puedes administrar Jenkins. Este lab se enfoca en la infraestructura de Google Cloud, no en la administración de Jenkins, por lo que el propósito de este paso es ver que el menú esté disponible.
2. Deja abierta la ventana del navegador con el servicio de Jenkins. La usarás en la próxima tarea.

### Revisa la implementación y establece una conexión a la VM a través de SSH

1. En el menú de navegación, ve a **Compute Engine > Instancia de VM**.
2. Haz clic en **jenkins-1-vm**.
3. Haz clic en **SSH** para conectarte con el servidor de Jenkins.

**Nota:** La interfaz de la consola ejecuta varias tareas de manera tra

```bash

# DETENEMOS EL SERVICIO DE JENKINS
sudo /opt/bitnami/ctlscript.sh stop

# REINICIAMOS EL SERVICIO DE JENKINS
sudo /opt/bitnami/ctlscript.sh restart
```

## Demo Projects
```bash

# Muestra una serie de configuraciones del project
gcloud config list

# Agarra un atributo de la lista de configuraciones, esto para ver en el project que estoy actualmente
gcloud config list | grep project

# Esto nos permite cambiar de project a otro
export projectId = OTRO_ID_PROJECT
gcloud config set project $projectId
```

## Virtual Private Cloud (VPC)
- Con Google Cloud , puedes aprovisionar tus recursos de Google Cloud, conectarlos entre si y aislarlos unos de otros en una nube privada virtual
- Tambien podemos definir  politicas de red
- VPC es un conjunto integral de objetos de red administrados por Google
- *VPC Objects*:
	- Los *projects* abarcara todos los recursos que usamos , incluidos los servicios de redes.
	- Existen 3 tipos de *networks*: predeterminado, modo automatico y modo personalizado
	- Las *subnetworks* permiten dividir y segregar el entorno 
	- Las *regions* y *zones* representan los centros de datos de google
	- VPC proporciona direcciones IPs para uso interno y externo


## Projects, networks y subnetworks
- Los *projects:*
	- Los projects son claves para la organizacion de los recursos de la infra de google 
	- asocia objetos y servicios de facturacion
	- La cuota predeterminada para cada project es de *15 networks*, pero puede solicitar una cuota adicional
- Los *networks*:
	- Se pueden compartir con otros projects
	- No tienen rangos IP
	- Son globales y abarcan todas las regiones del mundo. Es decir, podemos tener una red que este disponible para todo el mundo.
	- Dentro de esa network, podemos segregar recursos en *subnetworks*
	- Existen varios tipos: *default, auto (automatico) y custom (personalizado)*

### 3 VPC networks types

![[Pasted image 20251102184139.png]]

- *Default*:
	- Cada project cuenta con una red VPC predeterminada con subredes y reglas de firewall preestablecidas.
	- Especficamente se asigna una *subnetwork* a cada region con bloques CIDR no superpuestos y reglas de firewall que permiten el trafico de entrada para ICMP, RDP y SSH desde cualquier lugar asi comoel trafico de entrada desde dentro de la red predeterminada para todos los protocolos y puertos.
- *Auto mode:*
	- Se crea una *subred* de cada region dentro de ella
	- La *network* predeterminada es en realidad una red en modo automatico
	- Las subredes  creadas automaticamente usan un conjunto de rangos IP con una mascara de /20 que se puede ampliar /16
- *Custom mode:*
	- Una red de este tipo, no crea subredes de manera automatica
	- Este tipo nos da el control de las subredes y sus rangos IPS
	- Usted decide que subredes crear, en las regiones elija y usando los rangos IPs que especifiquemos
	- Este rango IP no pueden superponerse entre subredes de la misma red


![[Pasted image 20251102184325.png]]
- Aca por ejemplo las VMs A y B pueden comunicarse mediante sus direcciones IP INTERNAS ya que estan en la misma subred, sin importar la region en la que esten se pueden comunicar directamente entre si.
- A diferencia de las VMs C y D, estas estan en subredes distintas incluso en la misma region estas DEBEN comunicarse mediante las IP EXTERNAS. El trafico entre estos pasa a traves de los enrutadores de *Google Edge*




![[Pasted image 20251102184853.png]]
- No pueden sobreponerse los rangos IPS con otras subnets
- El rango IP debe ser un bloque CIDR valido y unico
- No pueden abarcar rangos RFC ni direcciones IPS publicas para uso privado
- El rango nuevo debe ser mayor al anterior
- Los de modo automatico tienen un rango IP de /20 pero pueden expandirse hasta /16 (no mas)
- Evita crear subredes grandes

Para extender el rango IP de la subred debemos ir a ella y configurar manualmente:
![[Pasted image 20251102185927.png]]



## IP Addresses

![[Pasted image 20251102193119.png]]
- IP Interna
	- En Google Cloud, a cada maquina virtual se le puede asignar dos direcciones IP. Una es una direccion IP interna el cual es asignada por el DHCP
	- Cuando creamos una VM en Google Cloud, su nombre se registra en un servicio de DNS  interno, *donde traduce* el nombre a la direccion IP interna
	- El *DNS* tiene un alcance limitado de la red, por lo que podemos traducir  URL webs y nombres de las VMs de los hosts de la *misma red*
- IP Externa:
	- Puedes asignar una IP EXTERNA si tu dispositivo o maquina esta conectado a la red electrica externa
	- Esa IP externa puede asignarse desde un grupo o puede asignarle una direccion IP externa reservada, lo que la hace estatica
	- Si esta sin usarse la direccion reservada , entonces se la va a cobrar más
![[Pasted image 20251102193601.png]]



## Mapping IP Addresses

![[Pasted image 20251102193920.png]]
- Independientemente si se usa direccion efimera o estatica, la direccion externa es desconocida para el SO de la maquina virtual. La direccion externa se asigna a la direccion interna de *forma transparente* mediante VPC
- Ejemplo si corremos el *ifconfig* en la terminal de la VM, nos mostrara que la misma solo conoce la direccion interna y nunca la externa.

### Resoludor DNS para direcciones internas
![[Pasted image 20251102230422.png]]
- Google Cloud tiene 2 tipos de nombres DNS internos: DNS zonal y DNS global (para todo el proyecto). 
- Cada instancia tiene un nombre de host que se puede resolver en una direccion IP interna.
	- El nombre de host es el mismo que el nombre de la instancia
	- Tambien existe un nombre de dominio interno completamente calificado para una instancia que usa el formato que se muestra en la diapositiva
- Cada instancia tiene un servidor de metadatos que tambien actua como un resolvedor DNS para esa instancia
- El servidor de metadatos gestiona todas las consultas DNS para los recursos de la red local y dirige todas las demas consultas a los servidores DNS publicos de Google para la resolucion de nombres publicos
- La red almacena *una tabla de busquedas que relaciona las direcciones IP externas con las direcciones IP internas* de las instancias correspondientes.


## Cloud DNS
- Es un servicio de sistemas de nombres de dominios autoritativo, escalable, fiables y gestionado que  se ejecuta en la misma infraestructura que Google
- Traduce las solicitudes de nombre de dominio como *google.com* en direcciones IP
- Proporciona una baja latencia ya que contiene ubicaciones redundantes en todo el mundo
- Alta disponibilidad 
- Permite crear y actualizar  millones de registros DNS sin la carga de gestionar tus propios servidores y software DNS.